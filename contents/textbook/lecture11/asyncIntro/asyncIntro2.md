<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Intro to Asynchronous Programming (Part Two)
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture11/asyncIntro/asyncIntro2.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

Original Credits go to [Prof Ooi's AY2017/2018 Semester 2 CS2030 Website](https://nus-cs2030.github.io/1718-s2/lec11/index.html)

## Recap

We recall that we have covered the concept of `CompletableFuture` in the [previous segment](asyncIntro.html)

## Functors and Monads with CompletableFuture

`CompletableFuture` is a functor.  Recall that a functor, in OO-speak, is a class that implements a (hypothetical) interface that looks like the following:

```Java
interface Functor<T> {
  public <R> Functor<R> f(Function<T,R> func);
}
```

In `CompletableFuture`, the method that makes `CompletableFuture` a functor is the `thenApply` method:

```Java
<U> CompletableFuture<U> thenApply(Function<? super T, ? extends U> func)
```

The method `thenApply` is similar to `thenAccept`, except that instead of a `Consumer`, the callback that gets invoked when the asynchronous task completes is a `Function`.  

There are other variations: 

- `thenRun`, which takes a `Runnable`, 
- `thenAcceptBoth`, which takes a `BiConsumer` and another `CompletableFuture`
- `thenCombine`, which takes a `BiFunction` and another `CompletableFuture` 
- `thenCompose`, which takes in a `Function` `fn`, which instead of returning a "plain" type, `fn` returns a `CompletableFuture`. 

All the methods above return a `CompletableFuture`.

By the way, `CompletableFuture` is a monad too!  The `thenCompose` method is analougous to the `flatMap` method of `Stream` and `Optional`. 

This also means that `CompletableFuture` satisfies the monad laws, one of which is that there is a method to wrap a value around with a `CompletableFuture`.  
We call this the `of` method in the context of `Stream` and `Optional`, but in `CompletableFuture`, it is called `completedFuture`.  This method creates a `CompletableFuture` that is completed.

The `completedFuture` method is useful, for instance, if we want to convert a method below to asynchronous.

```Java
Integer foo(int x) {
  if (x < 0) {
    return 0;
  } else {
    return doSomething(x);
  }
}
```
With `CompletableFuture`, it becomes:

```Java
CompletableFuture<Integer> fooAsync(int x) {
  if (x < 0) {
    return CompletableFuture.completedFuture(0);
  } else {
    return CompletableFuture.supplyAsync(() -> doSomething(x));
  }
```

<box type="warning">
    The following is an additional example covered in AY 2017/2018 Semester 2
</box>

In the class, I got carried away with the question about `completedFuture` and added the following example for `thenCompose` as well:

Original non-async version:
```Java
int x = bar(z);
int y = foo(x);
```

Async version:
```Java
y = barAsync(z)
        .thenCompose(i -> fooAsync(i))
        .get();
```

When we discussed about monad, we say that one way to think of a monad as a wrapper of a value in some context.  In the case of `Optional`, the context is that the value may or may not be there.  In the context of `CompletableFuture`, the context is that the value not be available yet.

Being a functor and a monad, `CompletableFuture` objects can be chained together, just like `Stream` and `Optional`.  We can write code like this:

```Java
CompletableFuture
    .completedFuture(Matrix.generate(nRows, nCols, rng::nextDouble))
    .thenApply(m -> m.multiply(m1))
    .thenApply(m -> m.add(m2))
    .thenApply(m -> m.transpose)
    .thenAccept(System.out::println);
```

Another example:

```Java
CompletableFuture left = CompletableFuture
    .supplyAsync(() -> a1.multiply(b1));
    
CompletableFuture right = CompletableFuture
    .supplyAsync(() -> a2.multiply(b2))
    .thenCombine(left, (m1, m2) -> m1.add(m2));
    .thenAccept(System.out::println);
```

Similar to `Stream`, some of the methods are terminal (e.g., `thenRun`, `thenAccept`), and some are intermediate (`thenApply`).

## Variations

- There are variations of methods with name containing the word `Either` or `Both`, taking in another `CompletableFuture`.  These methods invoke the given `Function`/`Runnable`/`Consumer` when either one (for `Either`) or both (for `Both`) of the `CompletableFuture` completes.

- There are variations of methods with name ending with the word `Async`.  These methods are called asynchronously in another thread

For example, `runAfterBothAsync(future, task)` would run `task` only after `this` and given `future` is completed.

Other features of `CompletableFuture` include:

- Some methods take in additional `Executor` parameter, for cases where running in the default `ForkJoinPool` is not good enough.

- Some methods takes in additional `Throwable` parameter, for cases where earlier calls might throw an exception.

## Handling Exceptions

Handling exceptions is non-trivial for asynchronous methods.  Remember that, in synchronous method calls, the exceptions are repeatedly thrown to the caller up the call stack, until someone catches the exception.  For asynchronous calls, it is not so obvious.  For instance, should we put a catch around `fork()` or around `join()`?  A `ForkJoinTask` doesn't handle exception with catch, but instead requires us to check for `isCompletedAbnormally` and then call `getException` to get the exception thrown.

As `CompletableFuture` allows chaining, it provides a cleaner way to pass exceptions from one call to the next.  The terminal operation `whenComplete` takes in a `BiConsumer` as parameter -- the first argument to the `BiConsumer` is the result from previous chain (or `null` if exception thrown); the second argument is an exception (null if completes normally).

```Java
CompletableFuture
    .completedFuture(Matrix.generate(nRows, nCols, rng::nextDouble))
    .thenApply(m -> m.multiply(m))
    .whenComplete((result, exception) -> {
        if (exception) { 
            System.err.println(exception);
        } else {
            System.out.print(result);
        }
    }
```
`whenComplete` returns a CompletableFuture, surprisingly, despite it taking in a `BiConsumer` -- in a sense, `whenComplete` is more similar to `peek` rather than `forEach`.

`handle` is similar to `whenComplete`, but takes in a `BiFunction` instead of a `BiConsumer`, thus allowing the result or exception to be transformed. 

Finally, `exceptionally` handles exception by replacing a thrown exception with a value, similar to `orElse` in `Optional`.


```Java
CompletableFuture
    .completedFuture(Matrix.generate(nRows, nCols, rng::nextDouble))
    .thenApply(m -> m.multiply(m))
    .exceptionally(ex -> Matrix.generate(nRows, nCols, () -> 0));
```

<box type="note">
    <code>CompletableFuture</code> is similar to <code>Promise</code> in other languages, notably JavaScript and C++ <code>std::promise</code>.
</box>

<box type="note">
    In Java, <code>CompletableFuture</code>  also implements a <code>CompletionStage</code> interface.  Thus, you will find references to this interface in many places in the Java documentation.  I find this name unintuitive and makes an already-confusing java documentation even harder to read.
</box>
