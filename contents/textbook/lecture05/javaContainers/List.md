<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# java.util.List<E\>
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture05/unboundWildcards/unboundWildcards.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->


#### Java's generic Collection type List<E\>

A List is a ordered Collection allowing duplicate elements with undefined length. 
handy classes that implement List<E\> includes: 
- ArrayList<E\>, which is implemented by resizing array. 
- LinkedList<E\>, which is implemented by chaining nodes.

The above two List classes have nearly no difference in usage but have differentiated performance features. (ArrayList is more prefered for large scale of data.)
**Constructor:**
- `ArrayList()` instantiates an ArrayList with initial capacity of ten.
- `ArrayList(int capacity)` instantiates it with specified capacity.
- `ArrayList(Collection<? extends E> c)` instantiates it with an existing Collection of subtypes of E.

**Basic methods:**
- `add(E e)` appends an element to the tail of list.
- `add(int index, E e)` inserts an element to specified index position.
- `get(int index)` returns the element on the specified index position.
- `remove(int index)` removes the element on the specified index position.
- `remove(Object o)` removes the first occurence of the argument if it presents.
- `set(int index, E e)` set the element of the specified index position as the given argument.
- `clear()` removes all the elements in the list.
- `size()` returns current number of elements the list contains.
- `contains(Object o)` returns a boolean value indicating if the argument is in the list.
- `indexOf(Object o)` returns index of the first occurence of the argument, or -1 if the list does not contain the argument.
- `toArray(T[] a)` returns an array of specified type containing all the elements, if the specified type is incompatible, ArrayStoreException will be thrown.

**Iterator of List:**
Use `.iterator()` to aquire an Iterator<E\> of the list.
An iterator must implement two methods: `hasNext()` and `next()`. 
We can thus use an iterator to access elements of the list sequentially
```java
List<Integer> list = new ArrayList<>();
for (int i=1; i<6; i++) list.add(i);
Iterator<Integer> iterator = list.iterator();
while (iterator.hasNext())
    System.out.print(iterator.next());
// Output: 12345
```






[For full information, please reffer to Java SE 11 & SDK 11](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/List.html)

  

