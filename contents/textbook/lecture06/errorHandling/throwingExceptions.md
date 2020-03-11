<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Throwing Exceptions
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture06/errorHandling/throwingExceptions.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

This section is a follow up to [Exception Handling](
https://github.com/purfectliterature/1920-s2/blob/master/contents/textbook/lecture06/errorHandling/errorHandling.html).
After seeing how we handle exceptions, now maybe we wish to throw exceptions 
ourselves. In this section, we will explore how to **manually throw exceptions**.

To throw exceptions, first of all, we must notify that our method is capable of
throwing such error. Let's say we have this program which calculates the sum of
two integers.
```java
public class AnotherDayAnotherClass {
    public int sum(int i1, int i2) {
        return i1 + i2;
    }
}
```
Really simple code. What if a user inputs a `null` object? Maybe we wish to throw
a `NullPointerException`. To do this, first modify the function signature.
```java
import java.util.NullPointerException;

public class AnotherDayAnotherClass {
    public int sum(int i1, int i2) throws NullPointerException {
        return i1 + i2;
    }
}
```
Then, we can use an if-clause to check if the inputs are valid.
```java
import java.util.NullPointerException;

public class AnotherDayAnotherClass {
    public int sum(int i1, int i2) throws NullPointerException {
        if (i1 == null || i2 == null) {
            throw new NullPointerException("Don't anyhow please");
        }
        
        return i1 + i2;
    }
}
```
Now, if any of `i1` or `i2` are `null` objects, the **method** (not class!) will
throw a `NullPointerException` with a message `Don't anyhow please`.
