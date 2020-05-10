<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Nested Classes in Java
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture06/nestedClasses.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

## Definition of Nested Classes

Nested Classes are defined as classes which are defined in another class. There are two types of nested classes, one is static nested classes, and the other is non-static inner classes. Inner classes are further classified into local classes and anonymous classes

1) static nested class : A nested class that is declared static is called static nested class.
2) inner class : An inner class is a non-static nested class

### static nested classes

As with class methods and variables, a static nested class is associated with its outer class. And like static class methods, a static nested class cannot refer directly to instance variables or methods defined in its enclosing class: it can use them only through an object reference. They are accessed using the enclosing class name:

```java
OuterClass.StaticNestedClass
```

1) static nested classes can only access static members of enclosing class

2) top-level class cannot be made static

##### Example of static nested class

```java
class Circle {
    private double radius;
    public static String color = "green";
    public Circle() {
        new CircleCreator().create(this);
    }

    private static class CircleCreator {
        private void create(Circle circle) {
        ...
    }
}
```

In this example, the static nested class CircleCreator can only access the static field color, and it cannot access radius

### Inner class Type 1: local inner classes

Local Inner Classes are defined inside a block, which is usually a method body. This block can be a for loop as well. They belong to the block of code that they are defined within. These class have access only to the fields of the enclosing class and fields of the block which are final or explicitly final. Local inner class must be instantiated in the block which they are defined in.

1) Declaring a local inner class: A local inner class can be declared within a block. This block can be either a method body, a for loop etc.

2) Accessing members: A local inner class has access to fields of the class enclosing it and the fields of the block that it is defined within which are final or explicitly final

#### Example of local inner class

```java
class B {
    void f() {
        int x = 0;
        class A {
            int y = 0;
            A() {
                y = x + 1;
            }
        }
        A a = new A();
    }
}
```

[![Explanation.jpg](https://i.postimg.cc/L6HKHXyL/06guide.jpg)](https://postimg.cc/G9SSKcnm)

In this example, as x is explicity final here(because it does not change), thus A class can access x (Java Memory Model is shown in the picture, adopted from recitation 6)

### Inner class Type 2: Anonymous Inner Classes

Anonymous Inner Classes are inner classes without a name and for which only a single object is created. The example of ananymous inner classes that we have learned in lectures is lambda expressions and implementing functional interface

There are three types of anonymous inner classes in total:

1) Anonymous Inner class that extends a class

2) Anonymous Inner class that implements a interface

3) Anonymous Inner class that defines inside method/constructor argument

#### Example of anonymous inner class

```java
Function<String,Integer> findFirstSpace = new Function<>() {
    @Override
    public Integer apply(String str) {
        return str.indexOf(' ');
    }};
```

This is an anonymous class which is equivalen to the following lambda expression:

```java
Function<String,Integer> findFirstSpace = str -> str.indexOf(' ');
```

## Summary

In summary, to conclude the concepts of nested classes, I recommend this diagram below adopted from GeeksforGeeks which shows the relationships between different nested classes clearly:

[![Nested Classes.jpg](https://i.postimg.cc/MpQh5QXk/d3.jpg)](https://postimg.cc/yD7QYWFn)
