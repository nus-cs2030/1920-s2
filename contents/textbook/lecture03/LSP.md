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







Thank you for reading! Please leave me feedback on how I can improve. 
