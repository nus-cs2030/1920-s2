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

4.flatMap(Function<? super T,? extends Optional<? extends U>> mapper)
Let's say you have a optional object in a optional object.
flatMap is useful when you you want to get rid of the outer Optional casing of a Optional and inner optional, apply a given function,
and putting it back into a Optional. The null will be passed on in the flatMap too and we do not need to
check for null in every step.

5.map(Function<? super T, ? extends U> mapper)
Map just takes the value in one optional casing and apply the function and returns the Optional of the applied value.

6.orElseThrow()
for throwing NoSuchElementException

7.ifPresentOrElse(Consumer<? super T> action, Runnable emptyAction)
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

