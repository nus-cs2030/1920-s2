<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Assertions
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture06/errorHandling/errorHandling.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

"A good code is a bug-free code," some might say. Though, it is almost 
impossible to write a bug-free code. What we can do is minimise the 
occurrence of errors, and if they do arise, handle them appropriately.
In this section, we will see the use of **assertions** in achieving so.

According to [this Java SE documentation](
https://docs.oracle.com/javase/7/docs/technotes/guides/language/assert.html),
> An *assertion* is a statement [...] to test your **assumptions** about
> your program. [...] Writing assertions while programming is one of the 
> quickest and most effective ways to detect and correct bugs.

**Assumptions** here refer to those expressions which are given by a problem.
For example, a problem set can say something like *"assume n > 0"* or 
*"assume the input length is always positive"*. In this case, it might seem
pointless to check for the assumptions, as (in CS1010 modules or CS1101S,)
we usually assume these assumptions **must always be true**.

However, when we are programming at a much higher and more complex
environment, assumptions may not hold across our code flow. Imagine we have
multiple functions, which work together in a sequence to provide a certain
functionality. Each functions may have their own respective assumptions.
However, for whatever reasons, **it is possible that the output of one 
function did not meet the assumptions of the input of the next function.**
This is where assertions come in.

If the assumptions were not met, and our code crashes, that's great!
Remember,
> we very much prefer codes to crash at compile time, rather than during
> runtime. This is because, runtime error are usually hard to trace and
> may happen unexpectably. If we receive an error during compile time, we 
> can debug and rectify the issue.
However, consider the case that given that the output of a function fails
to meet the assumptions of the next function, **and the function works** and
still give us a value. This is **what we try to prevent**, because results
which violate our assumptions are NOT valid results! It is for these reasons,
we use assertions such that we are able to catch these **bugs** and make
appropriate amendments.

To make an assertion, we use the `assert` keyword followed by a logical test.
For example,
```java
public int factorial(int n) {
    assert n >= 0;
    int output = 1;
    // factorial logic code here
    return output;
}
```
When the logical test in the assertion returns `false`, the compiler throws 
an `AssertionError`. We also have another type of assertion with the structure 
`assert (a) : (b)` like so,
```java
public int factorial(int n) {
    assert n >= 0 : "n: " + n;
    int output = 1;
    // factorial logic code here
    return output;
}
```
The second statement `(b)` is a value which will be printed together with the
thrown `AssertionError`. This is useful with codes with multiple assertions, 
and allow us to directly see which assertion was broken, with which value.
Note that assertions are **not meant for the end user**. Assertions are meant 
for developers, like us, in debugging our code. Therefore, this message need 
not be intuitive or user-friendly. This message should be informative, succint,
and contextual to the developers.

If the assertions are complex and resource-expensive to evaluate (e.g. sorting
or searching with `O(n^2)` complexity), then assertions will definitely hurt 
our code's performance. Therefore, by default, Java **disables** assertions
evaluation. So, when our code is run, these assertions are just ignored. To
enable assertions, we have to explicitly tell `java` that we want to see these 
`AssertionError`s by running our code with
```
java -ea <ClassName>
```
where the `-ea` flag stands for `e`nable `a`ssertions. To disable assertions,
you've guessed it, we can use `-da`, which stands for `d`isable `a`ssertions.

Again, note that **assertions are never meant for the end user**. Therefore, 
when releasing our program (releasing means finalising and deploying our program
to the end user), we have to remove these assertions, since our finalised program
should not rely on `AssertionError`s to rectify bugs, i.e. they should be as
bug-free as possible.
