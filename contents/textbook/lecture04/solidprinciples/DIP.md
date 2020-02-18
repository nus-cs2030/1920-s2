<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Dependency Inversion Principle
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture04/solidprinciples/DIP.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

The Dependency Inversion Principle states that:

> High-level modules should not depend on low-level modules. Both should depend on abstractions.

> Abstractions should not depend on details. Details should depend on abstractions.

In other words, there should be no dependencies between the client and the implementer. Instead, the client should have a dependency on an interface and the implementer should be a subclass of that interface. This will allow the higher level modules (client) to be unaffected by changes to lower level modules (implementer).

