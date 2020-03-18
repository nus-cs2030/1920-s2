<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Lecture 3 Summary
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture03/summary/chapter-summary.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

## Substitutability in Object Oriented Programming

"Composition" describes a "has-a" relationship, while
inheritance describes an "is-a" relationship.

In jshell, the order in which you declare your Java files matters.
Declare the class with no dependencies first, then move up.

A misconception I had long been guilty of was cleared up.
I assumed that when I did the following :
```Java
Circle c = new Circle(2);
c = new FilledCircle(4, Color.BLUE);
```
I would be able to call c.getColor() (which is a method
declared only in the FilledCircle child and not in the 
Circle parent). 

But that's not the case. As long as the variable is declared
as type Circle, it can only call its own methods and access its
own properties, and cannot automatically access those of its children.

A child class can substitute a parent class, but a parent class cannot substitute
a child class.

However, there are cases when my child class **overrides** a method 
in my parent class. Let's say that method is getMe() :

```Java

class Circle {
	private final double radius;
		Circle(double radius) {
		this.radius = radius;
	}

	double getArea() {
		return Math.PI
		* radius
		* radius;
	}

	double getPerimeter() {
		return 2 * Math.PI * radius;
	}

	@Override
	public String toString() {
		return "circle: area " + String.format("%.2f", getArea()) +
		", perimeter " + String.format("%.2f", getPerimeter());
	}

	public String getMe(){
		return "I'm a Circle";
	}
}


import java.awt.Color;
class FilledCircle extends Circle {
	private final Color color;

	FilledCircle(double radius, Color color) {
		super(radius);
		this.color = color;
	}

	Color getColor() {
		return color;
	}

	@Override
	public String toString() {
		return super.toString() + ", " + getColor();
	}

	@Override 
	public String getMe(){
		return "I'm a Filled Circle";
	}
}
```

Then if I execute the following code,

```Java
Circle c = new Circle(2);
c = new FilledCircle(4, Color.BLUE);
System.out.println(c.getMe());
```

The system will print "I'm a Filled Circle".

---

```Java
Circle c = new FilledCircle(4, Color.BLUE);
FilledCircle fc = new Circle(4);
```
Line 1 is allowed, but not line 2.

The same concept can be demonstrated by the Java instanceof keyword.

```Java
new FilledCircle(2, Color.BLUE) instanceof Circle;
new Circle(2) instanceof FilledCircle;
```
The first line returns true, while the second line returns false.

But what happens in the following situation? :

```Java
Circle c = new FilledCircle(4, Color.BLUE);
c instanceof FilledCircle;
```
The code will return true. 

That means that even though the variable type that new FilledCircle(4, Color.BLUE)
is assigned is Circle, c's "core identity" will always be FilledCircle.

This is also the reason why type casting works the way it does :

```Java
Circle c1 = new Circle(3)
Circle c2 = new FilledCircle(3, Color.BLUE)

(FilledCircle)c1
(FilledCircle)c2
```
The first cast returns an error, while the second cast is legal. 

Quote from an instructor :
>These objects are stored in a memory region known as the heap.
In your code, these objects are assigned
to a variable whose type is Circle, but
that does not change the fact that the objects
are FilledCircle and Circle respectively.
Therefore, the compiler is able to check if
the type casting is legal based on the objects stored in the heap memory.

In more formal terms, Circle is the "compile-time type" of c2, 
while "FilledCircle" is the "run-time type" of c2

---
Consider the following code :

```Java
for (Circle circle : circles) {
    if (circle instanceof FilledCircle) {
        System.out.println((FilledCircle) circle);
    } else if (circle instanceof Circle) {
        System.out.println((Circle) circle);
    }
}
```
A subtlety to take note of here is the order of the if statements.
If the second statement checking if circle is an instance of Circle 
came first, it would always return true, since both FilledCircle and
Circle objects are instances of Circle.

```Java
for (Circle circle : circles) {
    System.out.println(circle);
}
```

What is the difference between the first for loop and the second for loop?
In the first for loop, the code makes it clear to the compiler which toString()
method I want to call for each System.out.println call. This is called static binding.

In the second for loop, the "circle" array can throw any subclass of Circle
or a Circle object at the for loop; System.out.println doesn't know which version
of toString() it will have to call during execution. This is called dynamic binding,
and the beauty of it is that I can have as many subclasses of Circle I want without
changing the "client code" which is in this case the for loop. 

All I need to make sure is that the method
that will be dynamically called is overriden 
in all my child classes, like how toString() is overriden in the FilledCircle class.


---
## Type/Sub-type Consistency

The return type of a method that overrides a parent method can only have
a return type that is either the same as the return type of the parent method,
or more specific.

Similarly, access modifier of the method in the child class can only be equal
or more public, and never more private.

Side note: cleared a misconception that no access modifier defaults to public.
It actually defaults to the "default" access modifier, meaning it is only visible to 
the package it is in.

How about the parameter type in the overriding method? Take, for example, 
parent class A and child class B.

```Java
class A{
    Number foo(Number x){
        System.out.println("A");
        return null;
    }
}

class B extends A{
    @Override
    Number foo(Integer x){
        System.out.println("B");
        return null;
    }
}
```
This is not legal. The compiler will return an error saying that 
"method does not override or implement a method from a supertype". 
What if we replace the Integer type with Object? Wouldn't it now work since
Number or any of its subclasses can be passed into B's foo? 

The moment the parameter is of a different type as compared to the parent method,
method overloading takes place. More specifically, the moment the number, type, or
order of parameters are different, method overloading occurs. 

So if I remove the @Override annotation in the above code, I would be overloading the
foo method in B. Take note, however, that this new implementation will not work
the same as overriding. 

```Java
B b = new B();
b.foo(5);

A b = new B();
b.foo(5);
```
Here, only the first call to foo will use B's implementation of foo.
The second one will use A's implementation. If foo had been properly overriden in B,
and not overloaded, the second foo would also have called B's implementation.

## Polymorphism

Inheritance is used to support polymorphism.

```Java
class Animal {
	public void animalSound() {
		System.out.println("The animal makes a sound.");
	}
	public void animalLegs() {
		System.out.println("How many legs does this animal have?");
	}
}
	
class Dog extends Animal {
	public void animalSound() {
		System.out.println("This animal woofs.");
	}
	public void animalLegs() {
		System.out.println("This animal has 4 legs.");
	}
}

class Bird extends Animal {
	public void animalSound() {
		System.out.println("This animal chirps.");
	}
	public void animalLegs() {
		System.out.println("This animal has 2 legs.");
	}
}

class Snake extends Animal {
	public void animalSound() {
		System.out.println("This animal hisses.");
	}
	public void animalLegs() {
		System.out.println("This animal has no legs.");
	}
}

class MainAnimal {
	public static void main(String[] args) {
		Animal animal = new Animal();
		Animal dog = new Dog();
		Animal bird = new Bird();
		Animal snake = new Snake();
	
		animal.animalSound();
		dog.animalSound();
		bird.animalSound();
		snake.animalSound();
	
		animal.animalLegs();
		dog.animalLegs();
		bird.animalLegs();
		snake.animalLegs();
	}
}
```
This is the beauty of polymorphism - during compile time, animal variable is of type Animal, in runtime it is of type Dog (this transition from animal to dog is part of polymorphism).
Imagine if polymorphism doesn't exist, you have to create multiple overloaded animalSound() and animalLegs() methods to take in different animals such as animalSound(Dog d), animalSound(Bird b).
