<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br>

# Functional Interfaces

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture06/functionalInterfaces/functionalInterfaces.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
**Single Abstract Methods**
- Previously, we have had come across lambda expressions of type Function, Predicate, Supplier, Consumer, etc.
- Such interfaces are representative of what the key concept that we are going to be learning here - *Single Abstract Method(SAM)*
- As inferred from its name, these methods are representative of an anonymous class that implements any interface with only 1 abstract method.
- Why only one abstract method? It is to ensure that the compiler can infer which method body the lambda expression implements.
- By convention, we use a **lambda** expression(->) as a shorthand to represent them and functional interfaces in general.

The `Consumer<T>`, `Function<T, R>`, and `Predicate<T>` lambdas that we may have seen earlier fall under `java.util.function`

- Examples of functional interfaces:
```java
interface Predicate<T> { 
  boolean test(T t); 
}
interface Consumer<T> {
    void accept(T t);
    Consumer<T> andThen(Consumer<? super T> after);
}
interface Supplier<T> {
    T get();
}

class Foo {
  Predicate<Integer> p = x->x > 3; // define a predicate
  
  // print out all integers in range [0, 10) that are greater than 3.
  for (int i = 0; i < 10; ++i) {
    if (p(i)) {
      System.out.println(i);
    }
  }
}


/* The above code is equivalent to the following */
import java.util.function.Predicate; // using java function library
class Foo {
  Predicate<Integer> p = x->x > 3;
  
    for (int i = 0; i < 10; ++i) {
    if (p(i)) {
      System.out.println(i);
    }
  }
}
```
- Use of lambda expression to represent functional interfaces:
  - Following the above example, the lambda expression can be written as follows.

 ```java
import java.util.function.Predicate; // using java function library
class Foo {
  Predicate<Integer> p = new Predicate<Integer> {
    @Override
    public boolean test(Integer t) {
      return t > 3;
    }
  };
  
    for (int i = 0; i < 10; ++i) {
    if (p(i)) {
      System.out.println(i);
    }
  }
}
 ```
Additionally we can annotate a class with @FunctionalInterface to hint our intentions to the compiler and let the compiler help us catch any unintended error.

We can define our own functional interface as well.


```java
@FunctionalInterface
interface GetGrade{
  A takeGrade();
}
GetGrade good = test -> test.takeGrade();
```
For more information on functional interfaces, check out java documentation [here](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/function/package-summary.html).
