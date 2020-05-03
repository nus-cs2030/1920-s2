<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# No more Null
<hr>

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture07/Optional/Optional.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

## The Perils of Null

> I call it my billion-dollar mistake. It was the invention of the null reference in 1965. At that time, I was designing the first comprehensive type system for references in an object oriented language (ALGOL W). My goal was to ensure that all use of references should be absolutely safe, with checking performed automatically by the compiler. But I couldn't resist the temptation to put in a null reference, simply because it was so easy to implement. This has led to innumerable errors, vulnerabilities, and system crashes, which have probably caused a billion dollars of pain and damage in the last forty years.
>
> -- <cite>Tony Hoare, inventor of ALGOL W</cite>

In Java, the [null literal](https://docs.oracle.com/javase/specs/jls/se11/html/jls-3.html#jls-3.10.7) `null` represents the null reference, which is used to denote the non-existence of an object. Just like how primitive types take on a default value if declared but uninitialised (`int` takes on the value `0`, `boolean` takes on the value `false`, etc), uninitialised reference types take on the default value of `null`. Although the existence of null references might seem innocuous, it is the cause of many bugs written in programming languages without null safety.

Java is a statically typed language, meaning that the types of all variables are known at compile time. This allows the Java compiler to catch certain bugs statically (before the program is run) rather than dynamically (during the execution of the program), thus providing certain guarantees about the behaviour of the program.

For instance, let's say I have the following code:
```
Integer i = 2030;
i.toUpperCase();
```
Since `i` is of type `Integer`, the type check will fail and the code will not compile. By catching such errors during compile-time, static type checking helps immensely in the writing of large and complex programs.

However, the use of null references results in a pitfall that is common to many programming languages. That is, accessing a member of a null reference will result in a null reference exception, known as a `NullPointerException` in Java.

Take the below code for example:
```
String s = null;
s.toUpperCase();
```
It compiles without any issues, but throws a `NullPointerException` when run. While the cause of the bug might seem obvious here, `NullPointerException`s can be tricky to debug in large, complex software.

## Null Safety

Null safety is a guarantee within an object-oriented programming language that no object references will have null or void values. This helps dramatically reduce the occurrence of `NullPointerException`s during the execution of programs.

Certain languages such as Kotlin have [null safety](https://kotlinlang.org/docs/reference/null-safety.html) built into the language. In Kotlin, the type system distinguishes between references that can hold null (nullable references) and those that cannot (non-null references). Calling a method or accessing a property on non-null references is guaranteed to always be safe. Should the programmer wish to use nullable references, they have to explicitly designate the variable as nullable.

Kotlin also has a [safe call operator](https://kotlinlang.org/docs/reference/null-safety.html#safe-calls), which can be used to safely call a method or access a property on a reference without encountering a `NullPointerException`. The safe call checks if the reference is a null reference before evaluating the expression, returning `null` if called on a null reference.

Although Java does not have null safety built into the language, and is unlikely to ever do so for backwards compatibility reasons, it is still possible to do away with nulls with the `Optional` class introduced in Java 8.

## Optional

In lecture 7, we learn about the maybe class and using optional to hide the null and alse make the code more elegant by removing the need
to check for null using if and else.

In the lecture notes:
```
public class Maybe<T> {
  private final T thing;
  
  private Maybe() {
    thing = null;
  }
  
  public Maybe(T thing) {
    this.thing = thing;
  }
  
  public static <T> Maybe<T> empty() {
    return new Maybe<T>();
  }
  
  @Override
  public String toString() {
    return "Maybe[" + (thing == null ? "empty" : thing) + "]";
  }
}
```

Instead of creating Maybe class, we can use java Optional class. When there is a null, the optional class will just contain the null
like a box.

Some useful methods include:
1.of(T value)
Putting the value in Optional

2.empty()
returning an Optional of empty

3.ofNullable(T value)
Returns an Optional containing the value if not null. Else, returns Optional that is empty.

4.`flatMap(Function<? super T,? extends Optional<? extends U>> mapper)`
Let's say you have a optional object in a optional object.
flatMap is useful when you you want to get rid of the outer Optional casing of a Optional and inner optional, apply a given function,
and putting it back into a Optional. The null will be passed on in the flatMap too and we do not need to
check for null in every step.

5.`map(Function<? super T, ? extends U> mapper)`
Map just takes the value in one optional casing and apply the function and returns the Optional of the applied value.

6.orElseThrow()
for throwing NoSuchElementException

7.`ifPresentOrElse(Consumer<? super T> action, Runnable emptyAction)`
if value is present in the Optional, apply the action. or else perform the empty action.
For example:
```
//from recitation screencast of lab 6:
Student addModule(String code, String name, String grade) {
  this.get(code)
    .ifPresentOrElse(
      m -> m.addAssessment(name, grade),
      () -> this.put(new Module(code).addAssessment(name,grade)
      );
  return this;
}
 ```

Example of the usage of methods of Optional class as shown in Lab 6 Roster class (adapted from the recitation screencast):
```
String getGrade(String id, String code, String name) throws NoSuchRecordException {
  return this.get(id)
    .flatMap(Student -> student.get(code))
    .flatMap(module -> module.get(name))
    .orElseThrow(() -> new NoSuchRecordException(id, code, name))
    .getGrade();
}
```

Note: Avoid using methods such as get because it might invoke a NullPointerException. Also, do not use isPresent, isEmpty as it is
equivalent of checking for null and it defeats the purpose of using the optional class.

