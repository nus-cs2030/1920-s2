# SOLID Principles
In object-oriented computer programming, SOLID is an acronym for five design principles intended to make software designs more understandable, flexible and maintainable.

SOLID stands for:
* **S**ingle Responsibility Principle
* **O**pen-closed Principle
* **L**iskov Substitution Principle
* **I**nterface Segregation Principle
* **D**ependency Inversion Principle
 
# Single Responsibility Principle
The Single Responsibility Principle states that:

> A class should only have one reason to change.

In other words, a class should only have a single responsibility. 

For example, say we have a class Circle with three methods, getArea, getPerimeter and print.

```
class Circle {
    double radius;

    Circle(double radius) {
        this.radius = radius;
    }

    double getArea() {
        return Math.PI * radius * radius;    
    }
    
    double getPerimeter() {
        return 2 * Math.PI * radius;
    }

    void print() {
        System.out.println(“Area “ + getArea() + “ and perimeter “ + getPerimeter();
    }
}
```

The Circle class can be seen as having 2 separate responsibilities:
1. Circle related properties and methods.
2. Output redirection (where to print to).

This violates the Single Responsibility Principle as the class should only have one single responsibility. In this case, dealing with the circle’s properties is the main responsibility of the Circle class. Any secondary responsibilities (e.g. output redirection) should instead be abstracted away into separate classes. 

For example, the print method could return a string instead, and let the client implement the responsibility of output redirection (e.g. print to file instead of to the user’s screen).

```
class Circle {
    double radius;

    Circle(double radius) {
        this.radius = radius;
    }

    double getArea() {
        return Math.PI * radius * radius;    
    }
    
    double getPerimeter(){
        return 2 * Math.PI * radius;
    }

    String print() {
        return “Area “ + String.format(“%.2f”, getArea()) + “ and perimeter “ +
            String.format(“%.2f”, getPerimeter());
    }
}
```

# Open-Closed Principle
The Open-Closed Principle states that:

> A class should be open to extension but closed to modification. 

This means that a class should be easily extendable without having to modify the class itself. 

For example, say we have a class Rectangle that extends from a class Shape. Now we would like to create a class CalculateArea that calculates the total area of shapes in an array.

```
public class CalculateArea {
    public double Area(Shapes[] shapes) {
        double area = 0;
        for(Shape s : shapes) { 
            area += r.width * r.height;
        }
        return area;
    }
}
```

Now what if we had another class Circle that we would like to add to the array? We would have to modify the CalculateArea class for the program to be able to run correctly.

```
public class CalculateArea {
    public double Area(Shape[] shapes) {
        double area = 0;
        for (Shape s : shapes) {
            if (s instanceof Rectangle) {
                area += s.width * s.height;
            } else if (s instanceof Circle) {
                area += s.radius * s.radius * Math.PI;
            } 
        }
        return area;
    }
}
```

This violates the open-close principle as CalculateArea class isn’t closed to modification. We needed to modify it in order to extend it.

Instead, the responsibility for calculating the individual shape's area should be left to the shape class. In this way, we can extend a new shape (e.g. Triangle) without having to modify the client’s implementation of CalculateArea.

```
public class CalculateArea {
    public double Area(Shape[] shapes) {
        double area = 0;
        for (Shape s : shapes) {
            area += s.getArea();
        }
        return area;
    }
}
```


# Liskov Substitution Principle
The Liskov Substitution Principle states that:

> If S is a subclass of T, then an object of type T can be replaced by that of type S without changing the desirable property of the program.

As an example, if Filled Circle is a subclass of Circle, then anywhere where we can expect a Circle, we can replace it with a Filled Circle.

For example,  we may have a FormattedText class:

```
class FormattedText {
    public String text;
    public boolean isUnderlined;

    public void toggleUnderline() {
        isUnderlined = (!isUnderlined);
	}
}
```

The FormattedText class has the method toggleUnderline that toggles the variable isUnderlined between true and false.

Now suppose we have a class URL that extends FormattedText.

```
class URL extends FormattedText {
	public URL() {
		isUnderlined = true;
	}
	
	@Override
	public void toggleUnderline() {
		return;
	}
}
```

In the URL class, the method toggleUnderline overrides the method in the parent class. It has been modified to not toggle the variable isUnderlined between true and false.

However, this violates the Substitution principle as we cannot substitute FormattedText object for URL object, as doing so may result in a change in behaviour (unable to toggle underline) of the program.

# Interface Segregation Principle
The Interface Segregation Principle states that:

> A client should not be forced to depend on methods it does not use.

This means that:
1. Classes should not implement methods that they can’t
2. Clients should not know of methods they don’t need.

For example, we may have an abstract class Shape with methods to get shape properties as well as scale a shape.

```
abstract class Shape {
    abstract double getArea();
    abstract double getPerimeter();
    abstract Shape scale(double factor);
}
```

However, this would violate the Interface Segregation Principle as some classes might want to implement scale but are unable to implement getArea and getPerimeter (e.g. 3D Objects). In addition clients might need to implement getArea and getPerimeter without needing to implement scale.

It would be better to separate the the methods dealing with shapes properties from methods dealing with scaling into different interfaces to avoid violating the Interface Segregation Principle. 

```
interface Scalable {
    Scalable scale(double factor);
}	
```

```
abstract class Shape {
    abstract double getArea();
    abstract double getPerimeter();
}
```

This way, shapes can choose to implement scalable only if needed.

```
class Circle extends Shape implements Scalable {…}
```

# Dependency Inversion Principle
The Dependency Inversion Principle states that:

> High-level modules should not depend on low-level modules. Both should depend on abstractions.

> Abstractions should not depend on details. Details should depend on abstractions.

In other words, there should be no dependencies between the client and the implementer. Instead, the client should have a dependency on an interface and the implementer should be a subclass of that interface. This will allow the higher level modules (client) to be unaffected by changes to lower level modules (implementer).
