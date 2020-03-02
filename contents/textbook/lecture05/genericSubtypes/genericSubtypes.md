<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Generic Subtypes
<hr>

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture05/genericSubtypes/genericSubtypes.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

We know that **Integer** is a subtype of **Number**. Consider the following lines of codes
```java
  // Integer is a subtype of Object
  Number n = 1; // polymorphism here. okay.
  System.out.println(n); // 1
  
  // But ArrayList<Integer> is not a subtype of ArrayList<Number>
  ArrayList<Integer> lst = new ArrayList<Number>();
  // compilation error : incompatible types: ArrayList<Number> cannot be converted to ArrayList<Integer>
  
```

When we try to upcast ArrayList<Integer> to ArrayList<Number>, it triggers a compilation error "incompatible types".
This is because ArrayList<Integer> is NOT a subtype of ArrayList<Number>, eventhough Integer is a subtype of Number.

This error may seem strange. Why? 
```java
  List<Integer> intLst = new ArrayList<>();
  List<Number> numLst = intLst; // incompatible types: ArrayList<Number> cannot be converted to ArrayList<Integer>.
```

Why do we get a compilation error in line 2? Suppose it succeeds, and some other number (e.g. Double or Float) are added into intLst,
then intLst will get "corrupted" since it no longer only contains **Integers**, as references of **numLst** and **intLst** share the same value.

On the other hand, the following is valid.
```java
  // ArrayList is a subtype of List
  List<Number> lst = new ArrayList<>();
```
That is, ArrayList<Number> is a subtype of List<Number>, since ArrayList is a subtype of List and both have the same parametric type **Number**
