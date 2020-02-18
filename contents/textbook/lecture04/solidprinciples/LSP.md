<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Liskov Substitution Principle
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture04/solidprinciples/LSP.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

The Liskov Substitution Principle states that:

> If S is a subclass of T, then an object of type T can be replaced by that of type S without changing the desirable property of the program.

As an example, if Filled Circle is a subclass of Circle, then anywhere where we can expect a Circle, we can replace it with a Filled Circle.

For example,  we may have a FormattedText class:

```
class FormattedText {
    public String text;
    public boolean isUnderlined;

    public void toggleUnderline() {
        isUnderlined = (!isUnderlined);
	}
}
```

The FormattedText class has the method toggleUnderline that toggles the variable isUnderlined between true and false.

Now suppose we have a class URL that extends FormattedText.

```
class URL extends FormattedText {
    public URL() {
        isUnderlined = true;
    }
	
    @Override
    public void toggleUnderline() {
        return;
    }
}
```

In the URL class, the method toggleUnderline overrides the method in the parent class. It has been modified to not toggle the variable isUnderlined between true and false.

However, this violates the Substitution principle as we cannot substitute FormattedText object for URL object, as doing so may result in a change in behaviour (unable to toggle underline) of the program.

## Alternative Example 

Consider that we are operating an airline, and we have 2 types of aircraft, the `Aircraft` and the `BigAircraft`. An `Aircraft` can carry 150 passengers and fly 5000km, whereas a `BigAircraft` can carry 300 passengers and fly 10000km. Therefore, a `BigAircraft` can replace an `Aircraft`, but the opposite cannot be performed. 

```Java
class Aircraft {
    protected final int range;
    protected final int numOfPassengers;
    Aircraft(int range, int numOfPassengers) {
        this.range = range;
        this.numOfPassengers = numOfPassengers;
    }
    @Override
    public String toString() {
        /*assert statement checks if the statement is true. 
          If false, it will throw an error at runtime*/
        assert this.range < 5000;
        assert this.numOfPassengers < 150;
        return "Aircraft flying " + this.range + " km with " 
         + this.numOfPassengers + " passengers!";
    }
}
class BigAircraft extends Aircraft{
    BigAircraft(int range, int numOfPassengers) {
        super(range, numOfPassengers);
    }
    @Override
    public String toString() {
        /*assert statement checks if the statement is true. If false, 
         it will throw an error at runtime*/
        assert this.range < 10000;
        assert this.numOfPassengers < 300;
        return "Aircraft flying " + this.range + " km with " 
        + this.numOfPassengers + " passengers!";
    }
}
```
If I try to create a `Aircraft` object that flies `(3000, 100)`, the object will still be created. If either parameter goes above its limits however, the object will not be created.

LSP is demonstrated here because the `BigAircraft` class maintains the functionality of its parent class, while also adding its own functionality, hence it is substitutable for its parent class. 

