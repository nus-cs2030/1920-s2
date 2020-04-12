<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Variable Capture
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture07/VariableCapture/VariableCapture.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

For classes that are defined within a method, variable capturing will occur. Variable capture is when the local **variables** within the
local **function** in which the nested class is defined in, is being captured inside of the instance of the inner class in the heap.
Looking at the code from Recitation 6 Q2b,
```java
class B {
  void f() {
    int x = 0;
    class A {
      int y = 0;
      A() {
        y = x + 1;
      }
    }
    A a = new A();
  }
}
```
In this example, the variable int x is defined in the enclosing function in which the class A is defined. This means that the variable x
is a local variable, and will be captured inside every instance of A in the heap, even if not explicitly declared in A. This means that
methods in class A will be able to make use of the variable x.

[![image.png](https://i.postimg.cc/m2QgS11X/image.png)](https://postimg.cc/fJRszLKm)

*Variable x is captured in the instance of A inside the heap!*

## Qualified this
In order for us to refer to the enclosing class, we use a **qualified this**, syntaxed as `ClassName.this`.
In the example below, a **qualified this** is used to refer to a field in the enclosing class.

```java
class B {
  int x = 1;
  void f() {
    int y = 2;
    class A {
      void g() {
        B.this.x = y;
      }
    }
    A a = new A();
    a.g();
  }
}
```
`B.this.x` is the **qualified this** that refers to the variable x in the enclosing class, B. In contrast to the previous example (Recitation 6 Q2b) at the top of this page, the variable x is referenced through the **qualified this** and is not captured in the instance of A. We cannot simply use `this.x` because it will refer to the instance of A.

[![image.png](https://i.postimg.cc/YSVLDXQN/image.png)](https://postimg.cc/8FRCjmKs)

*`B.this` is stored in the instance of A inside the heap!*

## Effectively final
From java 8, local variables that are captured must be final or effectively final. In the first example above (Recitation 6 Q2b), this means that x must be final or effectively final. Since variable x is captured by A, trying to change the value of x will result in a compilation error.
```java
class B {
  void f() {
    int x = 0;
    class A {
      int y = 0;
      A() {
        y = x + 1;
      }
    }
    x = 1; //results in an error
    A a = new A();
  }
}
```
Somewhere that we often see this phenomenon is in lamba expressions. Like inner classes, lambda expressions also captures local
variables, which is why when we use a lambda expression inside a method, we can reference the variables that are defined in the
enclosing method within our lambda expression. Since local variables are captured by lambda expressions, this means that those
captured variables must also be final or effectively final. For example,
```java
class Test {
  int supplierAdd(int x, int y) {
    Supplier<Integer> s = () -> x + y; //variables x and y are captured by lambda
    x++; //this results in a compilation error
    return s.get()
  }
}
```
