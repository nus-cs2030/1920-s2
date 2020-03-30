<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Intro to Asynchronous Programming
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture04/solidprinciples/DIP.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

Original Credits go to [Prof Ooi's AY2017/2018 Semester 2 CS2030 Website](https://nus-cs2030.github.io/1718-s2/lec11/index.html)

## Synchronous vs. Asynchronous

In synchronous programming, when we call a method, we expect the method to be executed, and when the method returns, the result of the method is available.

```Java
int multiply(int x, int y) {
    return x * y;
}

int z = multiply(3, 4);
```

In the simple example above, our code continues executing after, and only after `multiply()` completes.

If a method takes a long time to run, however, the execution will delay the execution of subsequent methods, and maybe undesirable.

Asynchronous call to a method allows execution to continue immediately after calling the method, so that we can continue executing the rest of our code, while the long-running method is off doing its job.

You have seen examples of asynchronous calls: 

```Java
task = new MatrixMultiplyerTask(m1, m2);
task.fork();
```

The call above returns immediately even before the matrix multiplication is complete.  We can later return to this task, and call `task.join()` to get the result (waiting for it if necessary).  

## Future
Let's look at the `Future` interface a bit more.  `Future<T>` represents the result (of type `T`) of an asynchronous task that may not be available yet.  It has five simple operations:

- `get()` returns the result of the computation (waiting for it if needed).
- `get(timeout, unit)` returns the result of the computation (waiting for up to the timeout period if needed).
- `cancel(interrupt)` tries to cancel the task -- if `interrupt` is true, cancel even if the task has started.  Otherwise, cancel only if the task is still waiting to get started.
- `isCancelled()` returns `true` of the task has been cancelled.
- `isDone()` returns `true` if the task has been completed.

<box type="warning">
    Scala's <code>Future</code> is more powerful -- it allows us to specify what to do when the task completes, and it hands abnormal completions (e.g., exceptions). 
        Python 3.2 supports <code>Future</code> through <a href = "https://docs.python.org/3/library/concurrent.futures.html">concurrent.futures</a> module.  
        C++11 supports <a href = "(http://en.cppreference.com/w/cpp/thread/future">std::future</a> 
</box>

## CompletableFuture

We can try to check every second to see if the task is done. However, this **might not be optimal**.

For some applications, the response time is critical, and we would like to know as soon as a task is done.  For instance, response time is important in stock trading applications and web services.  

One way to do so, is to sleep for a shorter duration.  Or even not sleeping all together:

```Java
task.fork();
while (!task.isDone()) {
    System.out.print(".");
}
System.out.print("done");
```

This is problematic in many ways, besides printing out too many dots:

- this is known as _busy waiting_ -- and it occupies the CPU [Central Processing Unit](https://en.wikipedia.org/wiki/Central_processing_unit) while doing nothing.  Such code should be avoided at all cost. 
- we may want to continue doing other things besides printing out "."s, so the code won't be a simple for loop anymore.  We can do something like this instead:

```Java
task.fork();
if (!task.isDone()) {
    // do something
} else {
    task.join();
}
if (!task.isDone()) {
    // do something else
} else {
    task.join();
}
if (!task.isDone()) {
    // do yet something else
} else {
    task.join();
}
```

You can see that the code gets out of hand quickly, and this is only if we have one asynchronous call!

What we need is have a way to specify a _callback_.  A callback is basically a method that will be executed when a certain event happens.  In this case, we need to specify a callback when an asynchronous task is complete.  This way, we can just call an asynchronous task, specify what to do when the task is completed, and forget about it.  We do not need to check again and again if the task is done.

To do exactly this, Java 8 introduces the class `CompletableFuture<V>`, which implements the `Future<V>` interface.  Thus, just like `Future<V>`, a `CompletableFuture<V>` object returns a value of type `V` when it completes.  But `CompletableFuture<V>` is more powerful, it allows us to specify an asynchronous task, and an action to perform when the task completes.

The notion of "complete" is important for `CompletableFuture`.  If the `CompletableFuture` is complete, then the value to return is available.  We can create an already-completed `CompletableFuture`, passing in a value, or a yet-to-be-completed `CompletableFuture`, by passing in a function to be executed asynchronously.  When this function returns, the `CompletableFuture` completes.

To create a `CompletableFuture` object, we can call one of its static method.  For instance, `supplyAsync` takes in a `Supplier`:

```Java
CompletableFuture<Matrix> future = CompletableFuture.supplyAsync(() -> m1.multiply(m2));
```

As explained above, `future` completes when `m1.multiply(m2)` returns.  
Let's say that we want to print out the result with a `Consumer` when `future` completes, we can use the `thenAccept` method:

```Java
future.thenAccept(System.out::println);
```

Or, you can use the oneliner:

```Java
CompletableFuture
    .supplyAsync(() -> m1.multiply(m2))
    .thenAccept(System.out::println);
```

## Waiting for Completion

If you want your code to block until a `CompletableFuture` completes, you can call `join()`.  

```Java
m = future.join();
```

Suppose you have several `CompletableFuture` objects, say `cf1`, `cf2`, and `cf3`, and you want to block until all of these `CompletableFuture` completes.  You can create a composite `CompletableFuture` objects, using `allOf()`:

```Java
CompletableFuture.allOf(cf1, cf2, cf3).join();
```

The object created by `CompletableFuture.allOf(cf1, cf2, cf3)` completes, only after all of `cf1`, `cf2`, `cf3` completes.

There is also a `anyOf`, for cases where it is sufficient for any one of the `CompletableFuture` to complete:
```Java
CompletableFuture.anyOf(cf1, cf2, cf3).join();
```

To learn more, [click here](asyncIntro2.html)