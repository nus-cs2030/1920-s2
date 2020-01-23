<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Abstraction Vs Encapsulation
<hr>

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture01/abstractionVsEncapsulation/abstractionVsEncapsulation.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->



# Abstraction 

Abstraction is "the process of identifying common patterns that have systematic variations; an abstraction represents the common pattern and provides a means for specifying which variation to use" (Richard Gabriel).

Instead of making an object just suitable for the programmers themselves, what abstraction is trying to achieve is actually the opposite. Abstraction strives to make the object suitable for the domain in general. Programmers are able to use abstraction to construct a more generic solution that in turn solves more problems that have the same common pattern. This increases the re-usability of the components.

# Encapsulation

There are two aspects of encapsulation – packaging and information hiding.

Packaging: Related data or variable can be packaged in a self-contained unit. This can be done by using Classes to package low level data. As programmers, we need to be able to store respectively data in their related object or entity.

Information hiding: Multiple Object-Oriented languages enable programmers to hide information/data from the client and grant access only through the methods provided. In our case, Java, does it through the private and public access modifiers. A field or a method that is declared as private can only be accessed within the class itself, and cannot be accessed from outside the class. While, a public field or method can be accessed and even modified from outside of the class. By utilizing such mechanism, it maintains the abstraction barrier.

This might be able to better shed light on the accessibility of the modifiers:

|            | Class | Package | Subclass (same pkg)| Subclass (diff pkg)| World |
| ---------- |:-----:|:-------:|:------------------:|:------------------:|:-----:|
| public     | +     | +       | +                  | +                  | +     |
| protected  | +     | +       | +                  | +                  |       |
| no modifier| +     | +       | +                  |                    |       |
| private    | +     |         |                    |                    |       |

                   + : accessible            blank : not accessible
                   
# Example: The Student class

Let's see how the two aspect of encapsulation work to define a Student class in Java:

```
/**
 * A Student object encapsulates their name.  
 */
class Student {
  private String name;  // the name of the student  
  
  /**
   * Return the name of the student.
   */
  public double getName() {
    return name;
  }

  /**
   * Change the student’s name to a new name(newName)
   */
  public void setName(String newName) {
    name = newName;
  }
}
```

The variable name is declared as a private field inside the class Student. As such, they can only be accessed and modified with the class that it was encapsulated within (inside the abstraction barrier), such as in the methods getName and setName.
                

