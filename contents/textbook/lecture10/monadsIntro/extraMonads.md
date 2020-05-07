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

## Supplementary materials about pure functions

A function is called a pure function if it always returns the same result for same argument values and it has no side effects like modifying an argument (or global variable) or outputting something. The only result of calling a pure function is the return value. 

Examples of side effects:

1) Program input and output

2) Throwing exceptions

3) Modifying external state

#### Examples of impure functions

```java
int calculate(int x, int y) {
    return x / y;
}
```

Explanation: x / y may throw an java.lang.ArithmeticException when y = 0, thus this can cause side effects(point 2 in side effect list)

``` java
int foo(int i) {
    return this.count + i;
}
```

Explanation: the program makes use of this.count, which is a field of the class, thus the return value of the method does not always return the same result for same argument values. It might varies with different this.count value and thus it does not have a deterministic value.

```java
int addToQueue(List<Integer> queue, int i) {
    queue.add(i);
}
```

Explanation: this method changes the external state, it modifies the queue by adding in new values to queue, which is a list of Integer.

#### Example of pure function

```java
int p(int x, int y) {
    return x + y;
}
```

Explanation: this method does not have program input and output, it does not throw exceptions, and it does not modify external state. Thus it does have side effects. It always returns the same result for same argument values, thus it is considered as a pure function

## Supplementary materials about Functor

In functional programming, it is common to map objects and types to another type. One safe way of doing it is putting this object, stream, or data structure in a well-protected container which allows us to execute mapping operations with functions. Here is where Functors comes in.

Common examples of Functors which have been covered in CS2030 is Optional/InfiniteList. They have methods of the form: < R > Functor< R > map(Function< T, R > func)

Here is a simple implementation of a functor

```java
class FunctorExample {
    public double num;

    public FunctorExample(double num) {
        this.num = num;
    }

    public FunctorExample map(Function< Double, Double > func) {
        return new FunctorExample(func.apply(num));
    }

    @Override
    public boolean equals(Object o) {
        if (o == this) {
            return true;
        } else if (o instanceof FunctorExample) {
            FunctorExample temp = (FunctorExample) o;
            return this.num == temp.num;
        } else {
            return false;
        }
    }
}
```

A functor must obey the following two functor laws

1) Identity: if func is an identity function(such as x -> x), then it should not change the value of the functor.

2) Associative: if func is a composition of two functions(such as g ⋅ h), then the resulting functor should be the same as calling func with h and then with g.

Referring to the previous example, let us check if it obeys the functor laws.

```java
FunctorExample fe = new FunctorExample(1.0)
fe.equals(fe.map(x -> x)); // check Identity Law
fe.map(x -> x + 2.0).map(x -> x * 3.0).equals(fe.map(x -> (x + 2.0) * 3.0)); // check Associative Law
```

The result for the second line of code is true, and the result for the third line of code is true as well. Thus we can confirm that the above implementation is a functor because it obeys the two laws mentioned above


## How to build a monad

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
