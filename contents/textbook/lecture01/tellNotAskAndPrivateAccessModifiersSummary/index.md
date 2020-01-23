<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Tell, Don't Ask
<hr>

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture01/abstraction/abstraction.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

### 

In the lecture, the professor gave the following example :

```java
class Point {
    double x;
    double y;

    Point(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double distanceTo(Point otherpoint) {
        double dispX = this.x - otherpoint.x;
        double dispY = this.y - otherpoint.y;
        return Math.sqrt(dispX * dispX + dispY * dispY);
    }
}
```

The fields x and y are public, and therefore can be accessed like so :

```java {2-3}
Point p = new Point(1, 2);
p.x;
p.y;
```

Then the Circle class was introduced :

```java
class Circle {
    Point center;
    double radius;

    Circle(Point center, double radius) {
        this.center = center;
        this.radius = radius;
    }

    boolean contains(Point p){
        return this.center.distanceTo(p) < this.radius;
    }
}
```

If someone wanted to find out if a point was contained in a circle, 
he would type :

```java
Circle circle = new Circle(new Point(1, 2), 5);
Point myPoint = new Point(4, 5);
boolean circleContainsMyPoint = circle.contains(myPoint);
```

The above code is adhering to the "Tell Don't Ask" principle. In the Circle class, 
the contains method could have just as easily been :

```java
boolean contains(Point point) {
    double dx = center.x - point.x;
    double dy = center.y - point.y;
    return Math.sqrt(dx * dx + dy * dy) < this.radius;
}
```

In this case, Circle is asking Point for its x and y coordinates, then doing the 
square root calculations itself. Instead of **asking** for values and doing the 
work yourself like in this exmaple, classes should **tell** each other what to do in their own classes.

This brings up an important point. Let's say I changed the implementation of Point :

```java
class Point {
    double[] coord;

    Point(double x, double y) {
        this.coord = new double[]{x, y};
    }

    double distanceTo(Point otherpoint) {
        double dispX = this.coord[0] - otherpoint.coord[0];
        double dispY = this.coord[1] - otherpoint.coord[1];
        return Math.sqrt(dispX * dispX + dispY * dispY);
    }
}
```

If Circle had been adhering to "Tell Don't Ask", it wouldn't face any problems,
since it is not working directly with the values inside Point. But if it decides to
implement the contains method by directly asking for values from Point, it runs into some trouble since
point.x, center.y, etc. do not work anymore.

To prevent these sort of problems, classes can choose to hide their 
values using access modifiers. Outside classes will only be able to access
exposed functionalities, giving the original class freedom to change its implementation
however it likes.

```java
private double[] coord;
```

Getters can be used if a class wants to expose its fields without
making them public. As long as I write my Getters
to properly clean or transform my class fields for the outside world to access,
dependencies on my class will not break no matter my implementation.

Getters ensure that even if some classes are not following "Tell Don't Ask", 
they won't break when the implementation of the class they're "asking" from changes.

An excerpt from Stack Overflow summarizing the point :
>Private variables help prevent people from depending on certain parts of your code. 
For example, say you want to implement some data structure. 
You want users of your data structure to not care how you implemented it,
but rather just use the implementation through your well defined interface.
The reason is that if no one is depending on your implementation,
you can change it whenever you want. For example, you can change
the back-end implementation to improve performance. Any other
developers who depended on your implementations will break,
while the interface users will be fine. Having the flexibility
to change implementations without effecting the class users is
a huge benefit that using private variables
(and more broadly, encapsulation) gives you.

[This](https://softwareengineering.stackexchange.com/questions/143736/why-do-we-need-private-variables)
stack overflow thread contains other practical reasons for access modifiers.
