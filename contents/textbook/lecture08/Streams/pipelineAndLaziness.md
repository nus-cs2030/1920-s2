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
<br>

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
int sum = IntStream.iterate(1, 10).map(x -> x * 2).filter(x -> x % 2 == 0).forEach(System.out::println);
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
