<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Open-Closed Principle
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture04/solidprinciples/OCP.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

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