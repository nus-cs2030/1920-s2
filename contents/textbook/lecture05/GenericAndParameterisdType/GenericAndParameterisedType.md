<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br>

# Generic Type and Parameterised Type
<br>

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture05/GenericAndParameterisedType/GenericAndParameterisedtype.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

This was adapted from [Question 556](https://piazza.com/class/k54zo22zq1t2zc?cid=556) from Piazza.

Question:

- What is generic type and parameterized type referring to exactly?
For example, in the case of `Box<T>`, is `<T>` the parameterized type, and `Box<T>` itself the generic type?
Does "parameterized type" derive its name because it needs to be "parameterized", i.e. needs the type to be passed to it through instantiation or inference, etc?


Answer:

- Consider `class Box<T> { }`. `Box` is a generic type. `T` is a type parameter.
- Now consider `Box<Integer> box = new Box<>();`. `Box<Integer>` is a parameterised type. `Integer` is the type argument.
- We instantiate a parameterised type by replacing a type parameter of a generic type with a type argument.


To put it more specifically,
- A generic type is the declaration with formal type parameter(s).
- A parameterized type is the declaration with formal type parameter(s).
- A raw type is the declaration of a generic type with no actual type argument(s).
(reference from [stackoverflow](https://stackoverflow.com/questions/12551674/what-is-meant-by-parameterized-type))

Read more on Generic Types and Parameterised Types in the [Java Documentation](https://docs.oracle.com/javase/tutorial/java/generics/types.html)!