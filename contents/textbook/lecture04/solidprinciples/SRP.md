<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Single Responsibility Principle
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture04/solidprinciples/SRP.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

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