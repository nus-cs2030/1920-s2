<header>
  <navbar type="dark" class="temp-navbar">
    <a slot="brand" href="{{baseUrl}}/index.html" title="Home" class="navbar-brand">CS2030 AY19/20 Semester 2</a>
    <div class="nav-link temp-dropdown-placeholder">Peer Learning</div><dropdown text="Peer Learning" class="nav-link temp-dropdown">
      <li><a href="{{baseUrl}}/contents/peerlearning/peerlearning.html" class="dropdown-item">Introduction</a></li>
      <li><a href="{{baseUrl}}/contents/peerlearning/textbook.html" class="dropdown-item">Textbook Contributions</a></li>
      <li><a href="{{baseUrl}}/contents/peerlearning/peerlearningtask.html" class="dropdown-item">Peer Learning Tasks</a></li>
      <li><a href="{{baseUrl}}/contents/peerlearning/piazza.html" class="dropdown-item">Piazza Participation</a></li>
    </dropdown>
    <div class="nav-link temp-dropdown-placeholder">Guides</div><dropdown text="Guides" class="nav-link temp-dropdown">
      <li><a href="{{baseUrl}}/contents/guides/settingUpLabEnv.html" class="dropdown-item">Setting Up Lab Environment</a></li>
      <li><a href="{{baseUrl}}/contents/guides/settingUpJava.html" class="dropdown-item">Setting Up Java</a></li>
      <li><a href="{{baseUrl}}/contents/guides/settingUpVim.html" class="dropdown-item">Setting Up Vim</a></li>
      <li><a href="{{baseUrl}}/contents/guides/settingUpMacVim.html" class="dropdown-item">Setting Up MacVim</a></li>
    </dropdown>
    <li><a slot="brand" href="{{baseUrl}}/contents/textbook/textbook.html" class="nav-link">Textbook</a></li>
    <div class="nav-link temp-dropdown-placeholder">Links</div><dropdown text="Links" class="nav-link temp-dropdown">
        <li><a href="https://www.comp.nus.edu.sg/~cs2030/style/" class="dropdown-item" target="_blank">CS2030 Java Style Guide</a></li>
        <li><a href="https://www.comp.nus.edu.sg/~cs2030/javadoc/" class="dropdown-item" target="_blank">CS2030 Javadoc Specification</a></li>
        <li><a href="https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html" class="dropdown-item" target="_blank">JDK 11 Download Link</a></li>
        <li><a href="https://codecrunch.comp.nus.edu.sg/" class="dropdown-item" target="_blank">Codecrunch</a></li>
        <li><a href="https://github.com/nus-cs2030/1920-s2" class="dropdown-item" target="_blank">Github Repo</a></li>
        <li><a href="https://piazza.com/class/k54zo22zq1t2zc" class="dropdown-item" target="_blank">Piazza Forum</a></li>
    </dropdown>
    <li slot="right">
      <form class="navbar-form">
        <searchbar :data="searchData" placeholder="Search" :on-hit="searchCallback" menu-align-right></searchbar>
      </form>
    </li>
  </navbar>
</header>

<div id="content-wrapper">



<br> 

# Introduction to Imperative Programming
<hr>

## Definition
<div><div data-included-from="C:\Users\seanl\Documents\GitHub\1920-s2\contents\textbook\lecture01\imperativeProgramming\definition.md">

<!-- DO NOT DELETE THIS LINK --> 
[Edit the definition! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture01/imperativeProgramming/definition.md)
<!-- DO NOT DELETE THIS LINK --> 

Imperative programming specifies **how** computation proceeds using statements that change the state of the program. 



</div></div>

<br> 

## Explanation
<div><div data-included-from="C:\Users\seanl\Documents\GitHub\1920-s2\contents\textbook\lecture01\imperativeProgramming\explanation.md">

<!-- DO NOT DELETE THIS LINK --> 
[Edit the explanation! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture01/imperativeProgramming/explanation.md)
<!-- DO NOT DELETE THIS LINK -->

What we will be introduced in CS2030 is the concept of **Imperative Programming**. 
What we have learnt in CS1010J / CS1010S / CS1101S is the usage of **Functional Programming**, which is a form of **Declarative Programming**. 

Here some differences we can see in **Imperative Programming Vs. Functional Programming**. 

| Imperative | Functional | 
| :---------- | :---------- |
| Performing tasks and Changing program state | Information and Transformations to be required | 
| Mainly manipulated using classes and structures | Mainly manipulated using functions and data |
| Order of execution is important | Order of execution is not that important | 

A key difference to note is that in imperative programs, Each assignment updates or changes the value of a variable, 
allowing variable names to refer to different values at different points within the program itself. <br> 
Whereas in functional programs. Each function call should return a new value, 
and that value can be used again as an argument for another function call. Mutation of variables usually does not exit here.<sup>1</sup>

 
</div></div>

<br>

## Examples
<div><div data-included-from="C:\Users\seanl\Documents\GitHub\1920-s2\contents\textbook\lecture01\imperativeProgramming\examples.md">

<!-- DO NOT DELETE THIS LINK --> 
[Edit the examples! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture01/imperativeProgramming/examples.md)
<!-- DO NOT DELETE THIS LINK --> 

This is an example of an imperative approach: 

```java
class FoodChecker {
    int numOfExpiredFoods;

    public FoodChecker(List foods) {
        int numOfExpiredFoods = 0;
        
        for (food : foods) {
            if (food.isExpired()) {
                numOfExpiredFoods++;
            }
        }
    }

    public int getNumOfExpiredFoods() { 
        return this.numOfExpiredFoods;
    }   
}
```

This is an example of a functional approach: 

```java
class Main {
    public static void main(String[] args) {
        int numOfExpiredFoods = foods.stream().filter(food => food.isExpired()).count();
        
        System.out.println(numOfExpitedFoods);
    }
}
```
</div></div>

<br>

## Exercises
<div><div data-included-from="C:\Users\seanl\Documents\GitHub\1920-s2\contents\textbook\lecture01\imperativeProgramming\exercises.md">

<!-- DO NOT DELETE THIS LINK --> 
[Edit the exercises! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture01/imperativeProgramming/exercises.md)
<!-- DO NOT DELETE THIS LINK --> 

<question>
  Is Functional Programming a type of Declarative Programming or Imperative Programming? 
  <div slot="hint">
    Look at the top of the page!
  </div>
  <div slot="answer">
    Declarative Programming.
  </div>
</question>
</div></div>

<br>

## References
<div><div data-included-from="C:\Users\seanl\Documents\GitHub\1920-s2\contents\textbook\lecture01\imperativeProgramming\references.md">

<!-- DO NOT DELETE THIS LINK --> 
[Edit the references! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture01/imperativeProgramming/resources.md)
<!-- DO NOT DELETE THIS LINK --> 

1: https://sookocheff.com/post/fp/differences-between-imperative-and-functional/
</div></div>

<br>
</div>
<footer>
  <!-- Support MarkBind by including a link to us on your landing page! -->
  <div class="text-center">
    <small>Generated by <a href='https://markbind.org/'>MarkBind 2.8.0</a> on Mon, 13 Jan 2020 00:23:16 GMT</small>
  </div>
</footer>
