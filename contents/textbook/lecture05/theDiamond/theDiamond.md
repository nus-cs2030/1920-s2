<br> 

# The Diamond Operator
<hr>

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here!](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture05/staticGenerics/staticGenerics.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

You can replace the type arguments required to invoke the constructor of a generic class with an empty set of type parameters (<>) as long as the compiler can infer the type arguments from the context.  

```java
Box<Burger> boxofBurger = new Box();  // type: raw
Box<Burger> boxofBurger = new Box<Burger>();  // type: Burger
Box<Burger> boxofBurger = new Box<>();  // type: Burger
```

However, if we create a new object using the <> diamond operator without assigning it to another variable, the type argument is inferred to be of type Object.

```java
Box boxofBurger = new Burger<>();  // type taken to be Object
```  
