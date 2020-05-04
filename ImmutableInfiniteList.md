<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Immutable Infinite List

In Lecture 9, We learnt to appreciate the laziness of Streams by designing our own infinite list. 
1)	An infinite list (when not empty) contains a head value and a tail list.
2)	To make an infinite list be processed like a Stream, we can store the methods that are applicable to the infinite list in an IFL interface and create a class which implements the interface but behaves like a Stream, in a delayed manner.

##### Let’s create an immutable class which implements IFL<T>: IFLImpl< T >

##### IFLImpl< T > contains:
##### - A Head (`Supplier<Optional<T>>`)
##### -> Gives the current value when Supplier’s get() method is called.
##### (  ) -> O
##### - A Tail (`Supplier<IFLImpl<T>>`) 
##### -> Gives another IFLImpl when Supplier’s get() method is called.
##### (  ) -> OOOO

[![Infinite-List-Diagram.png](https://i.postimg.cc/8zj0rHnq/Infinite-List-Diagram.png)](https://postimg.cc/pmMZwDvZ)

###### Figure 1. An Infinite List with a head and a tail Infinite List which gives another head and tail Infinite List…

Both head and tail are Suppliers so that production of data can be delayed. Suppliers signal the intention that you want to do the action but not yet till you call Supplier’s get() method. Both head and tail should be made ‘**private final**’ in the spirit of Immutability. 

### Stream Operations
Different operations will be represented by different IFLImpl with its own customized operations on the head and tail. Additionally, every data source/ intermediate operation will generate a new node (IFLImpl) because here we are trying to make the nodes immutable rather than update the node.
<br>
#### Data Source Operations
##### Generate
```java
public static <T> IFLImpl<T> generate(Supplier<T> supplier) {
	Supplier<Optional<T>> newHead = () -> Optional.of(supplier.get());
	Supplier<IFLImpl<T>> newTail = () -> IFLImpl.generate(supplier);
    return new IFLImpl<T>(newHead, newTail);
}
```
##### Iterate
```java
public static <T> IFLImpl<T> iterate(T seed, Function<T, T> next) {
	Supplier<Optional<T>> newHead = () -> Optional.of(seed);
	Supplier<IFLImpl<T>> newTail = () -> IFLImpl.iterate(next.apply(seed), next);
	return new IFLImpl<T>(newHead, newTail);
}
```
Next seed is obtained via `next.apply(seed)`
<br>
#### Intermediate Operations 
##### Map
```java
public <R> IFLImpl<R> map(Function<T,R> mapper) {
	Supplier<Optional<T>> newHead = () -> IFLImpl.this.head.get().map(mapper);
	Supplier<IFLImpl<T>> newTail = () -> IFLImpl.this.tail.get().map(mapper);
	return new IFLImpl<T>(newHead, newTail);
}
```
Map returns a new IFLImpl object with the head supplier that
- is triggered by a terminal operation (e.g. forEach) at the end of the stream pipeline
- gets the element upstream from the data source through IFLImpl.this.head.get()
- apply the mapper function to the element obtained to get a mapped element
    
##### Filter
```java
public IFLImpl<T> filter(Predicate<T> predicate) {
	Supplier<Optional<T>> newHead = () -> IFLImpl.this.head.get().filter(predicate);
	Supplier<IFLImpl<T>> newTail = () -> IFLImpl.this.tail.get().filter(predicate);
	return new IFLImpl<T>(newHead, newTail);
}
```

Optional is used to deal with the missing values\
-> If a value is present, and the value  matches the given predicate, an Optional describing the value is returned.\
-> Otherwise, an empty Optional is returned.

##### Limit (state not maintained in the spirit of Immutability)
We can create an EmptyList class with an isEmpty() method that checks whether an infinite list is empty so that the terminal operations know when to stop the stream pipeline operation.

```java
public IFLImpl<T> limit(long n) {
    if (n <= 0 || isEmpty()) {
        return new EmptyList<>();
    }
    return new IFLImpl<T>(head, () -> { 
        if (n == 1) return head.get().isPresent()
            ? new EmptyList<>()
            : tail.get().limit(n);
        else return head.get().isPresent()
            ? tail.get().limit(n - 1)
            : tail.get().limit(n);
    });
}
```
<br>

#### Terminal Operations
##### ForEach

```java
public void forEach(Consumer<T> action) {
	IFLImpl<T> curr = this;
	while(true) {
		current.head.get().ifPresent(x -> action.accept(x));
		curr = current.tail.get();
	}
}
```
- `current.head.get()` invokes the head supplier to get an element
- `curr.tail.get()` will call iter which will return a new IFL
- forEach needs a reference to maintain what it is currently looking at, so we give it a current pointer (curr) that is originally pointing to itself and will refer to a new IFLImpl object everytime it is called
- forEach will call the newest node that has been generated

___
### **IMPORTANT CONCEPT**

### Variable Capture (Using Java Memory Model)
 
[![Variable-Capture.png](https://i.postimg.cc/8PqhtmC8/Variable-Capture.png)](https://postimg.cc/4mQ73ttw)

###### Figure 2. Model when IFL.iter(1, x -> x+1).forEach(action) is called and when we bring map into the picture

- Nodes don’t mutate throughout, a new IFL object is created
- Variables are captured within each IFL object created

#### Process:

1.	`IFL.iter(1, x -> x + 1)` creates a new IFLImpliter1 object with its specified head and tail. The seed(1) and next(i) is captured by the new object.
2.	forEach(action) is called on the object created. ‘this’ points to the object and curr also point to the object and the forEach method will operate on the object and call iter (refer to forEach section for more details). The cycle repeats until there is no element to retrieve.
3.	If map is called, map will create a new IFLImplmap object with its specified head and tail. The previous object will be captured to get the element from the object to apply mapper to it.
