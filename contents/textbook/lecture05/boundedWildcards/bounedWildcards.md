<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# bounded Wildcards
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture05/unboundWildcards/unboundWildcards.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

This is adapted from [The Java tutorial](https://docs.oracle.com/javase/tutorial/java/generics/upperBounded.html) and [Question 445](https://piazza.com/class/k54zo22zq1t2zc?cid=445) from Piazza.

> **upper-bounded wildcard**

`<? extends T>` is known as the upper-bounded wildcard. This is used to relax the use of complex type to its **co-variant complex types**, i.e.complex types with type parameter being subtype of `T`.  So, `List<? extends T>` means **a List of subtypes of T**. 
Short example:
consider the following `process` method:
```
public static void process(List<? extends T> myList) {
    for (T ele : myList) {
        // ...
    }
}
```
`myList` is the a list of **T** or **subtype of T**, thus no matter what runtime-type `ele` is, we can always assign compile time type **T** to it.

> **lower-bounded wildcard**

Similarly, `<? extends T>` is known as the lowwer-bounded wildcard, which relaxs the use of complex type to its **contra-variant complex types**, i.e.complex types with type parameter being supertype of `T`.  So, `List<? extends T>` means **a List of supertypes of T**.
Short example:
consider the following `adding` method:
```
public static void adding(List<? super T> myList) {
    myList.add(new T());
}
```
`myList` is the a list of **T** or **supertype of T**, thus it is always compatible to add object of type **T** into it since **T** is the subtype of **supertype of T**

#### Guidelines for Wildcard use
One of the more confusing aspects when learning to program with generics is determining when to use an upper bounded wildcard and when to use a lower bounded wildcard. This page provides some guidelines to follow when designing your code.

For purposes of this discussion, it is helpful to think of variables as providing one of two functions:

**An "In" Variable**
An "in" variable serves up data to the code. Imagine a copy method with two arguments: copy(src, dest). The src argument provides the data to be copied, so it is the "in" parameter.
**An "Out" Variable**
An "out" variable holds data for use elsewhere. In the copy example, copy(src, dest), the dest argument accepts data, so it is the "out" parameter.

Of course, some variables are used both for "in" and "out" purposes — this scenario is also addressed in the guidelines.

You can use the "in" and "out" principle when deciding whether to use a wildcard and what type of wildcard is appropriate. The following list provides the guidelines to follow:
> **Wildcard Guidelines:** 
 - An "in" variable is defined with an upper bounded wildcard, using  the extends keyword.
 - An "out" variable is defined with a lower bounded wildcard, using the super keyword.
 - In the case where the "in" variable can be accessed using methods  defined in the Object class, use an unbounded wildcard.
 - In the case where the code needs to access the variable as both an  "in" and an "out" variable, do not use a wildcard.




More quetions and exercises on [Java tutorial](https://docs.oracle.com/javase/tutorial/java/generics/QandE/generics-questions.html)