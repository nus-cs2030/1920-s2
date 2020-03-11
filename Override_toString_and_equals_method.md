<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Overriding toString Method
<hr>

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
    } else if (obj instance of) {
        Point p = (Point) obj;
        return Math.abs(this.x - p.x) < 1E-15 &&
            Math.abs(this.y - p.y) < 1E-15;
    } else {
        return False;
    }
}
````
