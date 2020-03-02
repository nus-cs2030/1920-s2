<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Wrapper Classes
<hr>

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture05/wrapperClasses/wrapperClasses.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

A wrapper class is a class that contains a primitive type, and allows us to use primitive types as objects. 
Since primitive types are not allowed as type arguments for generic classes, wrapper classes can be used in place of them.
Each primitive type has a corresponding wrapper class as shown in the following table.

| Primitive Type  | Wrapper Class  |
| --------------- | -------------- |
| boolean         | Boolean        |
| char            | Character      |
| byte            | Byte           |
| short           | Short          |
| int             | Integer        |
| long            | Long           |
| float           | Float          |
| double          | Double         |

<br> 

## Autoboxing
<br>

Autoboxing allows primitive types to be automatically converted into wrapper class objects.
When an instance of a wrapper class is expected and its corresponding primitive type is passed in instead, the primitive type is
automatically converted into an instance of the corresponding wrapper class.

Similarly, auto-unboxing allows instances of a wrapper class to be automatically converted into the corresponding primitive type.

Therefore, instead of writing:
```
Integer a = new Integer(123);
int b = new Integer(123).intValue();
```
We can write:
```
Integer a = 123;
int b = new Integer(123);
```

Although autoboxing is convenient, it comes with a cost to performance. Furthermore, wrapper classes are immutable, and this can lead
to many objects being created on the heap. Therefore, wrapper classes should only be used when necessary.
