<br> 

# Wrapper Classes
<hr>

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
<hr>

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
