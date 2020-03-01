The Java Language Specification defines a raw type as follows:

```
A raw type is defined to be one of:

1) The reference type that is formed by taking the name of a generic type declaration without 
an accompanying type argument list.

2) An array type whose element type is a raw type.

3) A non-static member type of a raw type R that is not inherited from a superclass
or superinterface of R.
```

A non-generic class or interface type is not a raw type.

An example to show the above definition:

```
class Outer<E> {
    class Inner { }
    static class Nested { }

    public static void main(String[] args) {
        Outer test;          // Warning: Outer is a raw type
        Outer.Inner inner;   // Warning: Outer.Inner is a raw type

        Outer.Nested nested; // No Warning: not parameterized type
        Outer<Object> test1; // No Warning: type parameter included
        Outer<?> test2;      // No Warning: type parameter included (wildcard)
    }
}
```

In the above example, `Outer<E>` is a parameterized type.

`test` has a raw type (generates a Compilation warning) by the first factor in the above definition; `inner` also has a raw type by the third factor.

`Outer.Nested` is not a parameterized type, despite being a member type of parameterized type `Outer<E>` . This is because it is static.

`test1`, and `test2` are both declared with type parameters, hence they are not raw types.

# What are raw types?

Raw types behaves just like they were before generics were introduced. Which means that the following example is entirely legal at compile-time.

```
List food = new ArrayList(); // Warning: Raw Type
names.add("Pizza");
names.add("Cake");
names.add(123); // No compilation error
```
The above code will run without errors.
However, that we also have the following:

```
for (Object o : food) {
    String food = (String) o;
    System.out.println(food);
} // throws ClassCastException error
  //    java.lang.Integer cannot be cast to java.lang.String
```

This however will generate an `ClassCastException` error at run-time because the list of `food` contains an object which in this case an `Integer` which is not a `String`.

Assuming that we want the list of `food` to contain only `String` which is the name of the `food`, we could still use a raw type and manually check every entry, and then manually cast to `String` every item from `food`. This is however, time-consuming and error prone. We could instead utilise Java generics in this case to let the complier to do all the work for us.

# How to suppress warnings due to Usage of Raw Type
Warning generated due to usage of raw type can be suppressed by implementing @SuppressWarning above the code.
Using the above example: 
```
@SuppressWarnings
List food = new ArrayList(); // Warning: Raw Type
names.add("Pizza");
names.add("Cake");
names.add(123); // No compilation error
```
However, it is not recommended as this could lead to potential run-time error.

# Why non-static type member of a raw type is considered raw?
To see why a non-static type member of a raw type is considered raw, consider the following example:

```
class Outer<T>{
    T t;
    class Inner {
        T setOuterT(T t1) {
          t = t1; 
          return t; 
          }
    }
}
```

The type of the member(s) of `Inner` depends on the type parameter of `Outer` . If `Outer` is raw, `Inner` must be treated as raw as well, as there is no valid binding for `T` .

This rule applies only to type members that are not inherited. Inherited type members that depend on type variables will be inherited as raw types as a consequence of the rule that the supertypes of a raw type are erased.

Another implication of the rules above is that a generic `inner` class of a raw type can itself only be used as a raw type:

```
class Outer<T>{
    class Inner<S> {
        S s;
    }
}
```

It is not possible to access `Inner` as a partially raw type (a "rare" type):

```
Outer.Inner<Double> x = null;  // illegal
```

Given that `Outer` itself is raw, hence so are all its inner classes including Inner, it is not possible to pass any type arguments to Inner.
It is a compile-time error to pass type arguments to a non-static type member of a raw type that is not inherited from its superclasses or superinterfaces.

Also:
```
Outer<Integer>.Inner x = null; // illegal
```
This is the opposite of the example discussed above. 
It is a compile-time error to attempt to use a type member of a parameterized type as a raw type

In conclusion : It is not recommended to use raw types unless you are absolutely certain in what you are doing. Unchecked errors can be suppressed by `@SuppressWarnings` . However, unforeseenable circumstances might lead to runtime errors as a result. It would be preferred to have complilation type error compared to runtime type error, so it is recommended not to suppress anyform or warnings.
