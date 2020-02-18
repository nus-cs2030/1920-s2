<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Interface Segregation Principle
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture04/solidprinciples/ISP.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

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