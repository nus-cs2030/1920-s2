<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Declaration of Generics in Static Methods
<hr>

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture05/staticGenerics/staticGenerics.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

This was adapted from [Question 521](https://piazza.com/class/k54zo22zq1t2zc?cid=521) from Piazza. 

Given the following set of code: 

```java
public class Box<T> {
    public static <T> Box<T>  of () {}; 
}
```

We discover that we will need to declare `<T>` once again. Is there a particular reason why `<T>` needs to be added before `Box<T>` for static methods as shown above? 

Well as we remmeber, tatic methods belong to the class, while non-static methods belong to the instance of the class. 

Different instances of the class Box, may have different type parameters `<T>`, i.e. Box of different types, `Box<Integer>`, `Box<String>` etc. However, static methods, as well as static variables, are **class level** so all instances of the class Box have access to the same static of() method. 

That **would not be possible** if the type parameter of the static method is referring to the class type parameter `<T>`, since `<T>` may differ across instances of the class Box. Therefore, what we need to do is to define a **local type parameter** specific to the scope of the static method. 

The type parameter `U`  created in static `<U>`, is then infered by Java for `U` in `Box<U>` ,U in argument (U u), and `U` in the return type of new `Box<U>()`, or just new `Box<>()` for short

In conclusion, as stated in the lecture note, the `<T> `in the static method **has no relation** with the class type `<T>` in `Box <T>`. 
