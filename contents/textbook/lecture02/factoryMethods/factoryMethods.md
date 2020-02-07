<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Factory Methods 
<hr> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture02/factoryMethods/factoryMethods.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->


## Definition of Factory Method:
"The Factory Method pattern is a design pattern used to define a runtime interface for creating an object. It's called a factory because it creates various types of objects without necessarily knowing what kind of object it creates or how to create it."
## Application in lecture notes:
The method createCircle was used to check the validity of the input parameters and prevent the creation of invalid objects.
```
static Circle createCircle(Point centre, double radius){
  if (radius > 0)
    return new Circle(centre, radius);
  else 
    return null;
}
```
**Note:** Factory methods is static. 

Static methods belongs to a class instead of an instance of the class.

According to my understanding, factory method is defined as static so that it can be called without the need of creating an instance.
## Advantages of Factory Methods:
1. Constructors cannot be named, while readable names can be assigned to factory methods.
2. Constructors can only return same type as the class name, however, factory methods can return other objects
3. Factory methods promote the idea of using interface instead of implementation, and would generate more flexible codes.

Please feel free to edit if I have made any mistakes or add on more content.:smile:
