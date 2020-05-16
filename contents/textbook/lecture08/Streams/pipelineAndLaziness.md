<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Streams
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture08/Streams/pipelineAndLaziness.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

Streams is a new type of data structure introduced to us in lecture 8. Unlike previous data structures, streams are mainly used for
functional programming, and we can often manipulate the data to get what we want in a single stream pipeline.

## Stream Pipeline
A stream pipeline consists of a data source(e.g. generate/iterate), zero or more intermediate operations(e.g. map, filter) and a 
terminal operation(e.g. forEach) to end the pipeline. Once a terminal operation is called, the stream is consumed, and can no longer be
used. An example of a stream pipeline:

```java
int sum = IntStream.range(1, 10).map(x -> x * 2).filter(x -> x % 2 == 0).sum();
//IntStream.range(1, 10) is the data source
//map and filter are intermediate operations
//sum() is the terminal reduction that ends the stream pipeline
```
## Laziness
What differentiates streams from other data structures is that streams are lazy. Laziness in streams means that in a stream pipeline,
intermediate operations are not evaluated until the terminal operation is called. Furthermore, the data from the data source is only
generated and consumed when needed. Another example:
```java
int sum = IntStream.iterate(1, x -> x + 1).map(x -> x * 2).filter(x -> x % 2 == 0).forEach(System.out::println);
//(x -> x * 2) is the Function<T,T> mapper
//(x -> x * 2) is the Predicate<T>
//note this program runs forever since iterate creates an infinite stream
```
In the above example, when we do create the source with iterate, we create a IntStream object in the heap, which captures the value of 
seed and the UnaryOperator next. The rest of the values in the stream is not yet generated. 
<br>
<br>
When we go to intermediate operations map and filter, we create a new IntStream object in the heap, that captures the the mapper 
function that we input into the map function and a pointer to the previous IntStream object. At this point, the function mapper is not
yet applied. Likewise for the filter operation, a new IntStream object is created in the heap, capturing the Predicate<T> and a pointer
to the previous IntStream object created by map. Every intermediate operation in our pipeline will create a new stream object in the heap that captures the operation we want to do and a pointer to the previous stream object.
<br>
<br>
Only when we call a terminal operation, in this case forEach, we follow the pointers upstream all the way to the source, get the current element, and apply all the intermediate operations downstream up to the terminal operation. We need to go through this upstream-downstream process for each element in the stream.
<br>
<br>
As in Prof Henry's lecture 9 slides, this example illustrates the laziness in streams.
```java
int sum = IntStream
  .rangeClosed(1, 10)
  .filter(
    x -> {
      System.out.println("filter: " + x);
      return x % 2 == 0;
    })
  .map(
     x -> {
      System.out.println("map: " + x);
      return 2 * x;
     })
  .sum();
System.out.println(sum);
```
This program prints:
```
filter: 1
filter: 2
map: 2
filter: 3
filter: 4
map: 4
filter: 5
filter: 6
map: 6
filter: 7
filter: 8
map: 8
filter: 9
filter: 10
map: 10
sum is 60
```
The output showing the filter and map interleaving with each other shows that the whole pipeline of intermediate operation is applied 
to one element at a time, as we would expect from the laziness of streams causing our evaluation process to go upstream and downstream
for each element and only generating the element and applying the operations as we need. For contrast, if we were to use a List for the
same program, the program would first filter the entire list, and then map the entire list, as opposed to applying the whole pipeline to
one element at a time.

## Advantages of Laziness
We can create incomplete stream pipelines with only intermediate operations and pass it around our program to be to continue adding more
intermediate operations or to be consumed at a later time.
<br>
<br>
In addition, since data elements are only generated and consumed when needed, we can create infinite streams. This is useful when we
want to do things where we don't know how many elements we need in our data structure. For example, we can write a program to find the 
first 100 prime numbers. If we were to use a list, we would not know how big our initial list needs to be. Using streams, we can simply
create an infinite stream, filter with a predicate to look for prime numbers and set a limit on the number of elements we want in the
stream.

## Converting Between Streams, Arrays and ArrayLists

From Array to ArrayList

```java
import java.util.Arrays;
import java.util.ArrayList;
import java.util.stream.IntStream;
import java.util.stream.Collectors;

/*
Integer[] -> ArrayList<Integer>
Works with any Object instead of Integer as well.
Does not work for primitive int[] array as Arrays.asList() method does not deal
with autoboxing and it will just create a List<int[]>
*/
Integer[] integerArray = {0, 1, 2, 3};
ArrayList<Integer> integerArrayList = new ArrayList<>(Arrays.asList(integerArray));

// int[] -> ArrayList<Integer>
int[] intArray = {0, 1, 2, 3};
// boxed method converts primitive int to Integer
ArrayList<Integer> intArrayList = IntStream.of(intArray).boxed() 
  .collect(Collectors.toCollection(ArrayList::new));
```

From Array to Stream

```java
// Integer -> Stream<Integer>
Integer[] integerArray = {0, 1, 2, 3};
Stream<Integer> integerStream = Arrays.stream(integerArray);
// int -> IntStream
int[] intArray = {0, 1, 2, 3};
IntStream intStream = Arrays.stream(intArray);
```

From ArrayList to Array

```java
// ArrayList<Integer> -> Integer[]
Integer[] integerArray = new Integer[integerArrayList.size()];
integerArray = integerArrayList.toArray(integerArray);
// ArrayList<Integer> -> int[]
int[] intArray = integerArrayList.stream().mapToInt(Integer::intValue).toArray();
// IntStream -> int[]
int[] intArray = intStream.toArray();
```

From ArrayList to Stream

```java
// ArrayList<Integer> -> Stream<Integer>
Stream<Integer> integerStream = integerArrayList.stream();
// ArrayList<Integer> -> IntStream
IntStream intStream = integerArrayList.stream().mapToInt(Integer::intValue);
```

From Stream to Array

```java
// Stream<Integer> -> Integer[]
Integer[] integerArray = integerStream.toArray(Integer[]::new);
// intStream -> int[]
int[] intArray = intStream.toArray();
```

From Stream to ArrayList

```java
// Stream<Integer> -> ArrayList<Integer>
List<Integer> integerList = integerStream.collect(Collectors.toList());
ArrayList<Integer> integerArrayList = new ArrayList<>(integerList);
// intStream -> ArrayList<Integer>
List<Integer> integerList = intStream.boxed().collect(Collectors.toList());
ArrayList<Integer> integerArrayList = new ArrayList<>(integerList);
```

## Functional Interfaces that stream operations can take in

### Predicate<T> 
    filter (Predicate <? super T> predicate)
    java.util.function.Predicate
    a simple function that takes a single value as parameter, and returns true or false

```java
public interface Predicate {
    boolean test(T t);
}	
```

The Predicate interface contains more methods than the test() method, but the rest of the methods are default or static methods which you don't have to implement.
You can implement the Predicate interface using a class, like this:

```java
public class CheckForNull implements Predicate {
    @Override
    public boolean test(Object o) {
        return o != null;
    }
}
```

You can also implement the Java Predicate interface using a Lambda expression. Here is an example of implementing the Predicate interface using a Java lambda expression:

```java
Predicate predicate = (value) -> value != null;
```

### Consumer<T>
    forEach (Consumer <? super T> consumer)
    java.util.function.Consumer
    a functional interface that represents a function that consumes a value without returning any value. 

```java
Consumer<Integer> consumer = (value) -> System.out.println(value);
```

### Supplier<T>
    generate (Supplier <? extends T> s)
    java.util.function.Supplier
    a functional interface that represents a function that supplies a value of some sorts.

```java
Supplier<Integer> supplier = () -> new Integer((int) (Math.random() * 1000D));
```

### Function<T, R>
    map (Function <? super T , ? extends R> mapper)
    java.util.function
    a function (method) that takes a single parameter and returns a single value
   
 ```java
public interface Function<T,R> {

    	public <R> apply(T parameter);
}
```

In the Function interface, the only method you have to implement is the apply() method. Here is a Function implementation example:

 ```java
public class IncreaseOne implements Function<Integer, Integer> {

 	   @Override
 	   public Integer apply(Integer a) {
  	      return a + 1;
 	   }
}
```

## Summary for Streams
Here is a Summary for Streams that you can use during finals since there are so many methods for streams. 

Do note that these are not exhaustive but what I feel is the most common(for this course that is).

| Obtaining a Stream | Intermediate Operations | Terminal Operations |
| --- | --- | --- |
|[`Stream.of(...)`](#streamof) | [`filter(Predicate <? super T> pred)`](#filter) | [`forEach(Consumer<? super T> action`](#foreach-consumer-action) |
| [`Stream.of(T t)`](#streamof) | [`map(Function<? super T, ? extends R> mapper)`](#map) | [`toArray()`](#converting-between-streams-arrays-and-arraylists) |
| [`Stream.generate(Supplier<? extends T> s)`](#streamgenerate) | [`flatMap(Function<? super T,? extends Stream<? extends R>> mapper)`](#flatmap) | [`noneMatch(Predicate<? super T> pred)`](#nonematch-allmatch-anymatchpredicate-super-t-predicate) |
| [`Stream.iterate(T seed, UnaryOperator<T> next)`](#streamiterate) | [`sorted()`](#sorted) | [`allMatch(Predicate<? super T> pred)`](#nonematch-allmatch-anymatchpredicate-super-t-predicate) |
| [`Stream.iterate(T seed, Predicate <? super T> hasNext, UnaryOperator<T> next)`](#streamiterate) | [`limit(long maxSize)`](#limitlong-maxsize) | [`anyMatch(Predicate<? super T> predicate)`](#nonematch-allmatch-anymatchpredicate-predicate) |
| [`Stream.concat(Stream<? extends T> a, Stream<? extends T> b)`](#streamconcat) | [`skip(long n)`](#skiplong-n) | [`count()`](#count) |
| [`IntStream.rangeClosed(int startInclusive, int endInclusive)`](#intstreamrangeclosed--intstreamrange) | [`distinct()`](#distinct) | [`min(Comparator<? super T> comparator)/ max(Comparator<? super T> comparator)`](#min-and-max) |
| [`IntStream.ranged(int startInclusive, int endInclusive)`](#intstreamrangeclosed--intstreamrange) | [`mapToObj/mapToInt/mapToLong`](#maptoobj-maptolong-maptodouble) | [`reduce(BinaryOperator<T> accumulator)`](#min-and-max) |
| | [`peek(Consumer<? super T> action)`](#peek-consumer-action) | 	[`reduce(T identity, BinaryOperator<T> accumulator)`](#reduce) | 
| | | [`collect(Collector<? super T,A,R> collector)`](#converting-between-streams-arrays-and-arraylists) | 

Also, the Stream API is [here](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/stream/Stream.html) and IntStream is [here](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/stream/IntStream.html)

#### Stream.of
Making a Stream out of individual values or an array or a list.
<br>

```java
//From a bunch of digits
Stream<Integer> stream = Stream.of(1, 2, 3, 5, 7, 11);

//From an array
Stream<String> stream = Stream.of(myArray)

// From a list (need to import java list)
list.stream(); // or from a list
```
<br>

[back to summary](#summary-for-streams)

#### Stream.generate
Returns an infinite sequential unordered stream where each element is generated by the provided Supplier.
Do note that these are infinite Streams and thus, would need a .limit() terminal function.

```java
// just some examples
// creating a strean of the words "test" 10 times. 
Stream<String> stream = Stream.generate(() -> "test").limit(10);

// creating a stream of random numbers
Stream.generate(new Random()::nextInt).limit(5).forEach(System.out::println);
```
<br>

[back to summary](#summary-for-streams)

#### Stream.iterate
Returns an infinite sequential ordered Stream produced by iterative application of a function f to an initial element seed, producing a Stream consisting of seed, f(seed), f(f(seed)), etc.
It takes in a seed and a unaryoperator

```java
// starts from 0
Stream<Integer> integers = Stream.iterate(0, n -> n + 1);

// starts from 2
IntStream.iterate(2, x -> x + 1)
```

Furthermore, there is also another type of iterate, where it can iterate and create a new element, but it has to satisfy the hasNext predicate. The first element will be the supplied seed value, the next element will be the result of applying the next function to the seed value, and so on iteratively until the hasNext predicate indicates that the stream should terminate. Do note if the seed does not pass the predicate, the stream will be empty. 

```java
Stream.iterate(1, i -> i <= 20, i -> i * 2).forEach(System.out::println); 
// output willl be 1,2,4,8,16

Stream.iterate(1, i -> i<0, i-> i-1).forEach(System.out::println);
// no output since there the first seed would not pass the predicate.
```
<br>

[back to summary](#summary-for-streams)

#### Stream.concat
Stream.concat method creates a concatenated stream in which the elements are all the elements of the first stream followed by all the elements of the second stream. The resulting stream is ordered if both of the input streams are ordered.

```java
// Creating 2 streams
Stream<String> stream1 = Stream.of("I", "hate"); 
Stream<String> stream2 = Stream.of("circuit", "breaker");

//Concating them and displaying the result
Stream.concat(stream1, stream2).forEach(element -> System.out.println(element)); 
// I
// hate
// circuit
// breaker
```
<br>

[back to summary](#summary-for-streams)

#### IntStream.rangeClosed / IntStream.range
IntStream range(int startInclusive, int endExclusive) returns a sequential ordered IntStream from startInclusive (inclusive) to endExclusive (exclusive) by an incremental step of 1.
<br>
IntStream rangeClosed(int startInclusive, int endInclusive) returns an IntStream from startInclusive (inclusive) to endInclusive (inclusive) by an incremental step of 1.

```java
IntStream stream = IntStream.rangeClosed(-1, 2);
// produces -1, 0, 1, 2
IntStream stream = IntStream.range(-1, 2)
// produces -1, 0, 1
```
<br>

[back to summary](#summary-for-streams)

#### filter
Returns a stream consisting of the elements of this stream that match the given predicate.

```java
Stream.of(1,2,3,4,5).filter(x -> x < 3);
// 1, 2

IntStream.rangeClosed(1, 10).filter(x -> x % 2 == 0).forEach(System.out::println);
// 2 4 6 8 10
```
<br>

[back to summary](#summary-for-streams)

#### map
Applying a function to each element

```java
IntStream.rangeClosed(1,4).map(x -> x + 1);
// 2 3 4 5 
```
<br>

[back to summary](#summary-for-streams)

#### maptoObj/ maptoLong/ maptoDouble
Do note that mapToObj is only present in IntStream/LongStream but not in the primitive Stream. 
This is because a Stream itself is already a stream of objects.
Use mapToX (mapToObj, mapToDouble, etc.) if the function yields Object, double, etc. values.

```java
// creates a bunch of circles as per lecture notes
Stream<Circle> circles = IntStream.rangeClosed(1, 3).mapToObj(Circle::new);

// From the midterms 2, mapping all the values in the rangeClosed to a String(you cant see the difference if you just print it)
IntStream.rangeClosed(1,3).mapToObj(x ->x + "").forEach(System.out::println);
```
<br>

[back to summary](#summary-for-streams)

#### flatmap
Stream flatMap(Function mapper) returns a stream consisting of the results of replacing each element of this stream with the contents of a mapped stream produced by applying the provided mapping function to each element. flatMap() is the combination of a map and a flat operation i.e, it applies a function to elements as well as flatten them.

```java

// Present in the lecture notes as well. 
IntStream.rangeClosed(1,3).flatMap(x -> IntStream.rangeClosed(1,x)).forEach(System.out::println);
//1 1 2 1 2 3
//
```
Similarly, there are also flatMaptoDouble/Int/Long. There isnt a flatmaptoObj but you can just do a workaround by combining mapToObj and flatMap.
<br>

[back to summary](#summary-for-streams)

#### sorted()
Stream sorted() returns a stream consisting of the elements of this stream, sorted according to natural order. For ordered streams, the sort method is stable but for unordered streams, no stability is guaranteed.
It can also take in a comparator to sort objects that are passed into Streams.

```java
IntStream.of(7, 9, 5, 2, 8, 4, 1, 6, 10, 3).sorted().forEach(System.out::println);
//sorts the stream accordingly 
```
<br>

[back to summary](#summary-for-streams)

#### limit(long maxSize)
Returns a stream consisting of the elements of this stream, truncated to be no longer than maxSize in length.

```java
IntStream.iterate(2, x -> x + 2).limit(5).forEach(System.out::println);
//produces 2 4 6 8 10
```
<br>

[back to summary](#summary-for-streams)

#### distinct()
distinct() returns a stream consisting of distinct elements in a stream. distinct() is the method of Stream interface. This method uses hashCode() and equals() methods to get distinct elements.

```java
IntStream.of(1, 1, 1, 0, 0, 0, 1, 0, 0, 1).distinct().forEach(System.out::println);
// 1 0
```
<br>

[back to summary](#summary-for-streams)

#### skip(long n)
The skip(long N) is a method of java.util.stream.Stream object. This method takes one long (N) as an argument and returns a stream after removing first N elements.

```java
IntStream.iterate(2, x -> x + 2).skip(2).limit(5).forEach(System.out::println);
// 6 8 10 12 14 (it skips the first 2 and 4)
```
<br>

[back to summary](#summary-for-streams)

#### peek(Consumer action)
Stream peek(Consumer action) returns a stream consisting of the elements of this stream, additionally performing the provided action on each element as elements are consumed from the resulting stream. This is an intermediate operation i.e, it creates a new stream that, when traversed, contains the elements of the initial stream that match the given predicate. Therefore, using peek without any terminal operation would do nothing. 
<br>
This method exists mainly to support debugging, where you want to see the elements as they flow past a certain point in a pipeline.

```java
/// no output because there is no terminal operation. 
IntStream.iterate(2, x -> x + 2).limit(5).peek(System.out::println);

// 
IntStream.iterate(2, x -> x + 2).limit(5).peek(System.out::println).count();
// 2 4 6 8 10 
// returns 5
```
<br>

[back to summary](#summary-for-streams)

#### 	forEach(Consumer action)	
Performs an action for each element of this stream. It is a terminal operation. 

```java
IntStream.range(4, 9).forEach(System.out::println); 
// 4 5 6 7 8 
```
<br>

[back to summary](#summary-for-streams)

#### noneMatch/ allMatch/ anyMatch(Predicate predicate)
These operations return a boolean result.
noneMatch returns true if none of the elements pass the given predicate
allMatch returns true if every element passes the given predicate
anyMatch returns true if at least one element passes the given predicate

```java
IntStream.range(2, 5).noneMatch(x -> 5 % x == 0); // true
IntStream.range(2, 5).allMatch(x -> 5 % x == 0); // false
IntStream.range(2, 5).anyMatch(x -> 5 % x == 0); // false

// if we replace 5 with 6
// we will get false false true. 
```
<br>

[back to summary](#summary-for-streams)

#### count()
Returns the count of elements in the stream as a long. It is a terminal operation

```java
IntStream.rangeClosed(1, 10).count();
// returns 10. Note that this 10 is of long type. 
```
<br>

[back to summary](#summary-for-streams)

#### reduce
reduce(T identity, BinaryOperator<T> accumulator)
<br>
Performs a reduction on the elements of this stream, using the provided identity value and an associative accumulation function, and returns the reduced value. First argument to reduce is the operation’s identity value, Second argument is the lambda that receives two
values and performs the accumulator onto them. 

```java
IntStream.rangeClosed(3,6).reduce(1, (x, y) -> x + y);
// this basically sums the entire thing up. BUT it starts with the seed of 1
// so it is 1+3+4+5+6
```
There is another version of reduce that does not take in an identity/seed. It performs a reduction on the elements of this stream, using an associative accumulation function, and returns an Optional describing the reduced value, if any.
<br>
NOTE: that this returns an Optional. This is because the calculation will start from the first element of the Stream(the first element will be treated as the seed). But there is also
the possibility that the stream is Empty. In that case, it will return an Optional. 

```java
IntStream.rangeClosed(3,6).reduce((x, y) -> x + y);
// this will produce an optional of 3 + 4 + 5 + 6 which is Optional(18)

//However, if we input an empty stream
IntStream.rangeClosed(3,3).filter(x -> x < 2).reduce((x,y)-> x+y)
// you will get an optional empty

//If there is only 1 element
IntStream.rangeClosed(3,3).reduce((x,y)-> x+y)
// you will get the only element, optional(3)

// useful way to get the last element of the stream
IntStream.rangeClosed(3,6).reduce((x,y)->y)
//Optional[6]
```
<br>

[back to summary](#summary-for-streams)

#### .min() and .max()
Stream.min() returns the optional containing the minimum element of the stream based on the provided Comparator. You can skip using a comparator if the stream is an IntStream as it will return according to the natural ordering. max returns the largest element.

```java
IntStream.rangeClosed(3,6).min()
// returns Optional[3]

// using a comparator for a normal stream
List<Integer> list = Arrays.asList(-9, -18, 0, 25, 4); 
Integer var = list.stream().min(Integer::compare).get(); //get used to get the number out of the optional.

// returns -18
```
<br>

[back to summary](#summary-for-streams)

#### Collect and toArray()
The uses of collect and toArray are explained in the converting from stream to array or stream to list. 
<br>

[back to summary](#summary-for-streams)

