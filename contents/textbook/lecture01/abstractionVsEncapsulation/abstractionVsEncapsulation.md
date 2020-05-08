<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Abstraction Vs Encapsulation
<hr>

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture01/abstractionVsEncapsulation/abstractionVsEncapsulation.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->



# Abstraction 

Abstraction is "the process of identifying common patterns that have systematic variations; an abstraction represents the common pattern and provides a means for specifying which variation to use" (Richard Gabriel).

Instead of making an object just suitable for the programmers themselves, what abstraction is trying to achieve is actually the opposite. Abstraction strives to make the object suitable for the domain in general. Programmers are able to use abstraction to construct a more generic solution that in turn solves more problems that have the same common pattern. This increases the re-usability of the components.

# Encapsulation

There are two aspects of encapsulation – packaging and information hiding.

Packaging: Related data or variable can be packaged in a self-contained unit. This can be done by using Classes to package low level data. As programmers, we need to be able to store respectively data in their related object or entity.

Information hiding: Multiple Object-Oriented languages enable programmers to hide information/data from the client and grant access only through the methods provided. In our case, Java, does it through the private and public access modifiers. A field or a method that is declared as private can only be accessed within the class itself, and cannot be accessed from outside the class. While, a public field or method can be accessed and even modified from outside of the class. By utilizing such mechanism, it maintains the abstraction barrier.

This might be able to better shed light on the accessibility of the modifiers:

|            | Class | Package | Subclass (same pkg)| Subclass (diff pkg)| World |
| ---------- |:-----:|:-------:|:------------------:|:------------------:|:-----:|
| public     | +     | +       | +                  | +                  | +     |
| protected  | +     | +       | +                  | +                  |       |
| no modifier| +     | +       | +                  |                    |       |
| private    | +     |         |                    |                    |       |

                   + : accessible            blank : not accessible
                   
# Example: The Student class

Let's see how the two aspect of encapsulation work to define a Student class in Java:

```
/**
 * A Student object encapsulates their name.  
 */
class Student {
  private String name;  // the name of the student  
  
  /**
   * Return the name of the student.
   */
  public String getName() {
    return this.name;
  }

  /**
   * Change the student’s name to a new name(newName)
   */
  public void setName(String newName) {
    this.name = newName;
  }
}
```

The variable name is declared as a private field inside the class Student. As such, they can only be accessed and modified with the class that it was encapsulated within (inside the abstraction barrier), such as in the methods getName and setName.
                
# Abstraction vs Encapsulation

|              |Abstraction                                 |Encapsulation                                 |
|:------------:|:------------------------------------------:|:--------------------------------------------:|
|Purpose       |To show relevant data and hide unwanted data|To hide the internal workings and data from the outside world|
|Implementation|Using abstract class and interface          |Wrap the data and code into a single unit using class and protect data using access modifiers (Public, Protected and Private)|

# Example: The Circle class

It could get confusing to differentiate between abstraction and encapsulation. The difference between the two is best shown using some examples to illustrate the transformation of a class that did not implement abstraction and encapsulation, to one that implements both abstraction and encapsulation. 

Suppose i have a circle class: 

```
/**
 * A circle class that contains x and y coordinates of the center point, and its radius.
 */
class Circle {
  double xCoord;
  double yCoord;
  double radius;

  //rest of the code is not shown for abrevity
}
```

In this Circle class, the center point is stored by its x and y coordinates. There are multiple issues using this approach, as this class violates both encapsulation and abstraction. 

Supppose i have another class CircleLocationTracker, which tracks the position of the center point of a Circle object, nothing is stopping me from doing:

```
class CircleLocationTracker {

  double[] location(Circle c1) {
    double [] location = new double [] {c1.xCoord, c1.yCoord};
    //
  }
  
}
```

What happens if we change the implementation of the center point such that it is stored a double array, as opposed to x and y coordinates? First, we have to change the implementation in the Circle class to store a double array containing the x and y coordinates. Next, we have to change the implementation of all classes, such as CircleLocationTracker, that directly access the x and y coordinates of Circle objects.

What happens if we change the implementation to using a List instead? You can see that it becomes increasingly troublesome whenever we change our implementation of the Circle class, and as we have more classes that use a Circle object. 

Let's first improve the design of this Circle class by adhering to the abstraction principle. We can do this by creating a Point class, which abstracts away the implementation details of a point.

```
class Point {
  double xCoord;
  double yCoord
  
  //rest of the code is not shown for abrevity
}

class Circle {
  Point centerPoint;
  double radius;

  //rest of the code is not shown for abrevity
}
```

Now, there is a HAS-A relationship between Circle and Point. Whenever we change the implementation of Point class, such as storing a double array as opposed to the x and y coordiantes, we only need to change the Point class. In this case, we can think of the Circle class as a client that uses the Point class, and the Point class is the implementor that implements the specific details of how a point should be modeled. The Circle class is not aware, and is not required to be aware of the implementation changes in the Point class, and all changes are abstracted away from the Circle class.

There are added benefits as well. Suppose we want to extend the capabilities of our Point class, such as including a distanceTo method that measures the distance between 2 points. We can now include this functionality in the Point class, as opposed to changing the Circle class in our initial implementation. 

However, encapsulation is not implemented yet. Nothing is preventing external classes from accessing and modifying the x and y coordinates stored within variable instance of type Point. For example, in my Circle class, i could be doing

```
class Circle {

  double radius;
  double xCoord;
  double yCoord;

  Circle(Point centerPoint, double radius) {
    xCoord = centerPoint.xCoord;
    yCoord = centerPoint.yCoord;
  }
}
```

The lack of encapsulation creates a "know-too-much implementation" implementation of the Circle class which can directly access the x and y coordinates of the Point class although the implementation detail is supposed to be abstracted away with the implementation of a Point class.

To tackle this, lets adhere to the encapsulation priciple and hides away the implementation details in the point class. 

```
class Point {
  private double xCoord;
  private double yCoord
  
  //rest of the code is not shown for abrevity
}

class Circle {
  Point centerPoint;
  double radius;

  //rest of the code is not shown for abrevity
}
```

Now, external classes are no longer able to access the xCoord and yCoord in the Point class. We have successfully transformed the circle class to one that adheres to both encapsulation and abstraction.

# Java Memory Model

Java Memory Model comprises three parts, which are Stack, Heap, and Non-heap(Metaspace since Java 8).

## Stack

- local variables and call frames(Primitive types and object references)

## Heap

- Dynamically allocated objects(Objects)

## Metaspace

- loaded classes and other meta classes
