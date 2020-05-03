<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Do you want to build a monad? 
<hr>

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture10/monadsIntro/extraMonads.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

This post was adapted from [Prof Ooi's CS2030 Website](https://nus-cs2030.github.io/1718-s2/monad/index.html) 

This post was inspired by [The Best Introduction to Monad](https://blog.jcoglan.com/2011/03/05/translation-from-haskell-to-javascript-of-selected-portions-of-the-best-introduction-to-monads-ive-ever-read/), but is adapted to the Object-Oritented way of programming with Java.

For a visual introduction to functors and monads, check [this](http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html) out!

Let's say we have:

```java
double sin(double x) {
  return Math.sin(x);
}

double cube(double x) {
  return x * x * x;
}
```

We can easily chain the methods together:
```java 
sin(cube(5.0));
cube(sin(5.0));
```

But, what if we need to print something while doing this operation? We can easily do: 
```java 
double sin(double x) {
  System.out.println("called sin");
  return Math.sin(x);
}

double cube(double x) {
  System.out.println("called cubed");
  return x*x*x;
}
```

But that has side effects, so it **violates the spirit of functional programming**. We should concat the logs into a string. 
So we need a class that encapsulates the variable with its log.

```java

class DoubleString {
  Double x;
  String log;

  DoubleString(double x, String log) {
    this.x = x;
    this.log = log;
  }
}
```

Now, we can write the methods as:

```java 
DoubleString sinAndLog(double x) {
  return new DoubleString(sin(x), "called sin");
}

DoubleString cubeAndLog(double x) {
  return new DoubleString(cube(x), "called cube");
}
```

In a way, we are writing methods that take in a value `(double x)` and add some context to it (the log). We wrap both the value and its context in a box (the `DoubleString`). But these new functions **do not compose** anymore. We cannot do `sinAndLog(cubeAndLog(x))`

We need methods that takes in a `DoubleString` and return a `DoubleString`

Great! Now we can write the methods to compose them:

```java
sinAndLog(cubeAndLog(new DoubleString(5.0,"")));
```

or in the **Object-Oriented Way:**

```java
  Double x;
  String log;

  DoubleString(double x, String log) {
    this.x = x;
    this.log = log;
  }

  DoubleString sinAndLog() {
    return new DoubleString(sin(this.x), log + "called sin");
  }

  DoubleString cubeAndLog() {
    return new DoubleString(cube(this.x), log + "called cube");
  }
}
```

In the **Object-Oriented Way**, we chain the methods together.

```java 
new DoubleString(5.0, "").sinAndLog().cubeAndLog();
```

So, how do we actually make it a monad? To find out more, [click here!](makingTheMonad.html)