<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>



<br>

# Functional Interfaces
<br>
- functional interface is an interface with only one abstract method. A lambda expression can be used to represent a functional interface. `java.util.function` has provided some common functional interfaces, for example `Consumer<T>`, `Function<T, R>`, and `Predicate<T>`.



- An example of a functional interface:

```java
interface Predicate<T> {
  boolean test(T t);
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



- For more information on functional interfaces, check out java documentation [here](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/function/package-summary.html).

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture06/functionalInterfaces/functionalInterfaces.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->