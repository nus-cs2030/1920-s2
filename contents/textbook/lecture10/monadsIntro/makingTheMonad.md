<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Do you want to build a monad? (Part Two)
<hr>

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture10/monadsIntro/makingTheMonad.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

This post was adapted from [Prof Ooi's CS2030 Website](https://nus-cs2030.github.io/1718-s2/monad/index.html) 

This post was inspired by [The Best Introduction to Monad](https://blog.jcoglan.com/2011/03/05/translation-from-haskell-to-javascript-of-selected-portions-of-the-best-introduction-to-monads-ive-ever-read/), but is adapted to the Object-Oritented way of programming with Java.

If you have not read the first part, [click here!](extraMonads.html)

## Making it a Monad

Let's refer back to our code from part one. 

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

Now, here is where I jump to creating a monad. I do not want to convert all my methods that takes in `double` and returns `DoubleString` into something that takes in `DoubleString` and returns `DoubleString`. Yet, I want to be able to compose them and chain them together. So I write a general method that allows that, and that is our `flatMap` method:

```java 
DoubleString flatMap(Function<Double, DoubleString> mapper) {
    DoubleString ds = mapper.apply(x);
    return new DoubleString(ds.x, this.log + "\n" + ds.log);
}
```

We can now use `flatMap` to chain different operations together.

```java 
DoubleString ds = new DoubleString(5.0, "")
    .flatMap(x -> sinAndLog(x))
    .flatMap(x -> cubeAndLog(x));
```

Now `DoubleString` is a **monad**!

Going back to our "wrap a value in a box with context" explanation. If we have two such wrappers, how do we wrap twice? 

We have to 

    (i) wrap it one time, 

    (ii) unwrap to get the new value and new context, and wrap it again.

The two lines in `flatMap` corresponds to (i) and (ii) respectively.

Line `ds = mapper.apply(x);` wraps it once; The next line unwraps the value and the context (`ds.x` and `dx.log`) and wraps it again ( `new DoubleString(..)` ) with the next context (`this.log + "\n" + ds.log`).

Here is our monad:

```java 
class DoubleString {
  Double x;
  String log;

  DoubleString(double x, String log) {
    this.x = x;
    this.log = log;
  }

  DoubleString flatMap(Function<Double, DoubleString> mapper) {
    DoubleString ds = mapper.apply(x);
    return new DoubleString(ds.x, this.log + "\n" + ds.log);
  }

  public String toString() {
    return x + "\n" + log;
  }
}
```

## Fulfilling the Monad laws

In order for a class to act as a Monad, it is not sufficient for it to merely represent a Monad syntatically by having a `flatMap` method or an `of()` method. It also has to behave the same semantically, by fulfilling certain laws, mainly the left identity law, the right identity law and the associative law. Let's dive deeper and check whether our DoubleString class fulfills these law. 

A Monad should have an unit operator, or more commonly seen as the `of()` method, which constructs a pure Monad with no context. Although our DoubleString class has no explicit `of()` method, the constructor can be seen as creating a pure DoubleString object with no context. We can achieve this by passing in **an empty String** into the constructor. So let's use the constructor with empty String as an unit operator that creates a pure DoubleString. 

Note that in the following laws, there is a difference between **M**onad (with capital M), and monad. A Monad is one that has a pure context, i.e. new DoubleString(x, ""), where log is an empty String, while a monad is one that already **has some context**, i.e. DoubleString(x, "divide").

Given functions `f denoted x → f(x)`, `g denoted x → g(x)`:

The **left identity law** states that `Monad.of(x).flatMap(f) ≡ f.apply(x)`. 

In this case, `new DoubleString(x, "").flatMap(x -> new DoubleString(x, ""))` is just `f.apply(new DoubleString(x, "").x)`, which returns us `DoubleString(x, "")`. 
`DoubleString(x, "")` is equivalent to our pure Monad, `new DoubleString(x, "")`. 
Hence, the left identity law holds.

The **right identity law** states that `monad.flatMap(x -> Monad.of(x)) ≡ monad`. 

Suppose we have `DoubleString object a`, which has some context in its String log, e.g. "divide". a is not a pure Monad as it already has some context.
`a.flatMap(x -> new DoubleString(x, "")) = new DoubleString(a.x, a.log + "") = a`. 
Hence, the right identity law holds.

The **associative law** states that `monad.flatMap(f).flatMap(g) ≡ monad.flatMap(x -> f.apply(x).flatMap(g))`. 

Suppose we have the same DoubleString object a, 
`a.flatMap(x -> new DoubleString(x1, "action1")).flatMap(x -> new DoubleString(x2, "action2"))` 
= `new DoubleString(x2, a.log + "action1" + "action2")`, 

which is equals to 

`a.flatMap(x -> f.apply(a.x).flatMap(x -> new DoubleString(x2, "action2")))` 
= `a.flatMap(x -> new DoubleString(x2, "action1" + "action2")` 
= `new DoubleString(x2, a.log + "action1" + "action2")`. 

Hence, the associative law holds.

## Making a Generic Monad that Logs

We can make `DoubleString` a **generic class** that logs what happen to a variable.

```java 
  T x;
  String log;

  Logger(T x, String log) {
    this.x = x;
    this.log = log;
  }

  <R> Logger<R> flatMap(Function<? super T, ? extends Logger<? extends R>> mapper) {
    Logger<R> ds = mapper.apply(x);
    return new Logger<>(ds.x, this.log + "\n" + ds.log);
  }

  public String toString() {
    return x + "\n" + log;
  }
}
```

## Functor

Can we do this with a functor? Note that a functor has a `map` method with type `Function<T,R>`. A `map` method for `DoubleString` would looks like `Function<Double,Double>`. So it cannot do what the monad does. A functor can only change the value inside the box, but it cannot rewrap it with an updated context.

## Wait, What is a Monad Again?

I hope the example above helps explain what is a monad. 

It is a **value wrapped in a box with context**, and it allows us to compose wrappers (wrap multuple times), operate on its value and update the context if needed.
