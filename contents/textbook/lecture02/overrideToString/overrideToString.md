<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Overriding toString Method
<hr>

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture02/overrideToString/overrideToString.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

By default, when a object is created, the hash code of the object is returned such as Classname@6a41eaa2 which is not meaningful. Thus, it is better to Override the toString method of the Object (every class by default inherits the toString method from class Object). Do take note that when we override, it has to be public.

For example:
```
public String toString() {
    return "Circle has a centre of" + this.centre + "with a radius of" + this.radius
}
```

Very often as observed in labs, the questions always ask us to return in 3 decimal places. Thus, here are some useful String formats you can use when you use the method String.format(_________):

%s is for string;
%f is for float;
%.3f is for float to 3 decimal places;
%d is for integers;

An example on how to use String.format will be :
``` 
public String toString() {
    return String.format("Today I folded %d paper cranes at a rate of %.3f cranes per hour", numOfCranes, rateOfFold)
}
```
where numOfCranes = 30, and rateOfFold = 5.9322
The return output of the String will be:
```
Today I folded 30 paper cranes at a rate of 5.932 cranes per hour
```

# Overriding equals Method
Classes inherit Object's equal method, which is not very useful because the method only implies if two objects' references to another obhject are the same or not. However, we want to use .equal() method to find equality. For example, lecture notes talk about how a Point is equal. It considers p.x and p.y and compares them with another point (this.x and this.y) in the given method below.

A more correct way of comparing two object method is via the equals method.
1. We have to check if the object is the same
2. We have to check if it is of the same type
3. We have to check if the values are equal (type casting is needed after checking if they are of the same type)

An example from the lecture notes is:

```
@Override
boolean equals(Object obj) {
    if (this == obj) {
        return true;
    } else if (obj instanceof Point) {
        Point p = (Point) obj;
        return Math.abs(this.x - p.x) < 1E-15 &&
            Math.abs(this.y - p.y) < 1E-15;
    } else {
        return False;
    }
}
````
Rules for overriding a method:
1. Only the overriding method is able to invoke the overriden method.
2. The access level of the overriding method cannot be more restrictive than that of the overriden method.
3. The return type of the overriding method should be the same or a subtype of the return type declared in the original overridden method in the superclass.
4. A method declared final cannot be overridden.
5. A subclass within the same package as the instance's superclass can override any superclass method that is not declared private or final.
6. A subclass in a different package can only override the non-final methods declared public or protected.
7. An overriding method can throw any unchecked exceptions, regardless of whether the overridden method throws exceptions or not. However, the overriding method should not throw checked exceptions that are new or broader than the ones declared by the overridden method. The overriding method can throw narrower or fewer exceptions than the overridden method.



# Method Overloading
Methods of the same name can co-exist if the signatures are different.

Examples of method signatures:
1. number of parameters.
2. order of parameters: foo(int a, double b); foo(double a, int b).
3. type of arguments.

Static binding occurs during method overloading. Method to be called is determined during compile time.
An example from the lecture notes is:

```
class A {
    Number foo(Number x) { ... }
    Number foo(String x) { ... }
}

A a = new A();
a.foo(123)

a.foo("123")
````
In contrast to method overriding, method overloading is very common among constructors. However, constructors cannot be overriden. 
A static method cannot be overriden but can be overloaded.




