A benefit of making objects immutable is that they can be used in a sort of "testing" framework. No matter how many "test methods" are 
applied to the object, it will come out the same at the end of the day. 

For exaple, if we structure our Point.java file like so :

```java
public class Point {

	private double x;
	private double y;
	
	public Point(double x, double y) {
		this.x = x;
		this.y = y;
	}
	
	public double getX() {
		return this.x;
	}
	
	public double getY() {
		return this.y;
	}
	
	public Point setX(double x) {
		return new Point(x, this.y);
	}
	
	public Point setY(double y) {
		return new Point(this.x, y);
	}
}
```
In this case, if a series of setX and setY are executed on a point, then that same series of methods calls will always give back
the same result when applied to the original point. One can envision how useful this might be in a testing context; I make one point
called p, then perform a series of sets on it to "transform" it, then call a "testing" function on it to test something.

I then want to change the conditions under which the testing function is called, so I pass in different values to
setX and do not setY altogether. Since p is immutable, however, I do not need to make a new Point p class in order to 
reset my y value - I can simply call setX on p, knowing that p has not changed state due to the previous state. 

My testing will look like a series of one-liner method-chains that are **independent** of one another.

"Bottom up testing" is recommended; start with zero-dependency classes and move up the ladder. But in order to do so,
you need to structure your code to be hierarchical and not cyclical.

Fascinating new concept of static "factory" methods. If we want
to check for the validity of the arguments to a constructor before actually deciding whether to make a class or not,
we can do the following :

```java
public class Point {

	private double x;
	private double y;
	
	private Point(double x, double y) {
		this.x = x;
		this.y = y;
	}
	
	public static Point createPoint(double x, double y) {
		if(x < 0 || y < 0) {
			return null;
		} else {
			return new Point(x, y);
		}
	}
}
```

Make the constructor private, then implement the createPoint factory method.

## Inheritance
Let's say I have a Circle class and a child UnitCircle class :

```java

public class Circle {
	protected Point center;
	protected double radius;

	public Circle(Point center, double radius) {
		this.center = center;
		this.radius = radius;
	}
	
	public static Circle getCircle(Point center, double radius) {
		if(radius <= 0) {
			return null;
		}
		Circle newCircle = new Circle(center, radius);
		return newCircle;
	}
}


public class UnitCircle extends Circle{
	public UnitCircle(Point center) {
		super.center = center;
		super.radius = 1;
	}
}
```

In this case, UnitCircle does not compile because it is not passing any
arguments into super() in its constructor.

>There is an implicit call to super() with no arguments for 
all classes that have a parent - which is 
every user defined class in Java - so calling
it explicitly is usually not required. However, 
you may use the call to super() with arguments 
if the parent's constructor takes parameters, and you wish to
specify them. Moreover, if the parent's constructor takes parameters, 
and it has no default parameter-less constructor, you will need
to call super() with argument(s).

So rather than manually getting the parent variables with super.center and
super.radius, we can just use the conventional method :

```java
public class UnitCircle extends Circle{

	public UnitCircle(Point center) {
		super(center, 1);
	}
}
```

Something insanely cool : you can automatically generate javadoc
documentation by inserting special comments into your java code.
[Here's](https://www.comp.nus.edu.sg/~cs2030/javadoc/) a quick guide
on the proper way to generate javadocs.

The purpose of "annotators" have finally been made clear :
if I add an "@Override" annotator above a method, an error will be called
if that method does not override a method from its parent class.
It basically acts as a checker to ensure that the method is doing
what I intend it to be doing. 

Below is the proper way to override the equals method (for the Point class)

```java
@Override
	boolean equals(Object obj) {
	if (this == obj) {
		return true;
	} else if (obj instanceof Point) {
		Point p = (Point) obj;
		return Math.abs(this.x - p.x) < 1E-15 &&
		Math.abs(this.y - p.y) < 1E-15;
	} else {
		return false;
	}
}
```

You can avoid cyclic dependency by adding another more "intangible" class
to solidify some sort of relationship between the two original classes.
E.g : add the "Loan" class in order to prevent cyclic dependency between the 
Student and Book class.
