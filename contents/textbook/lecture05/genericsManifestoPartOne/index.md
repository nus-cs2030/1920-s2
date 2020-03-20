The point of this "manifesto" series is to go a bit more in-depth
into fundamental generic concepts, and to share some subtle insights and
possibly confusing gotchas.

## 1. PECS
The PECS principle, which stands for Producer-Extends-Consumer-Super,
is a guideline to follow when using bounded wildcards.

Consider the classes `Vehicle`, `Car`, `Bus`, and `Roomba`, where `Car` and `Bus`
extends `Vehicle` and `Roomba` extends `Car`.

```java
List<? extends Vehicle> garage = new ArrayList<>();
garage.add(new Vehicle());  // compilation error
garage.add(new Car());      // compilation error
garage.add(new Bus());      // compilation error
garage.add(new Roomba());   // compilation error
```

The errors above seem at first to be uncalled for -
`garage` is a List of "any objects that extend Vehicle" (including
Vehicle itself), so it should be able to accept our Car, Bus,
and Vehicle objects.

What we're missing here is that `List<? extends Vehicle>`
doesn't actually mean a list that can contain any child objects of Vehicle.
`List<? extends Vehicle>` is in fact saying that it is a 
"list parameterized to one concrete class that extends Vehicle".

So `List<? extends Vehicle>` is one of `List<Car>`, `List<Truck>`, or `List<Roombar>`,
but the compiler does not know which. And because the compiler does not know which,
it does not allow the addition of, say, a Vehicle to `List<? extends Vehicle>` - what if the 
unknown identity of `List<? extends Vehicle>` is `List<Car>`? It wouldn't make
sense to add a Vehicle to a list of Cars. The same goes for adding a Car or Bus or Roomba
to `List<? extends Vehicle>`.

What if instead of trying to add to garage, we only want to get from it?
```java
List<? extends Vehicle> garage = new ArrayList<>(List.of(new Car(), new Bus()));

Vehicle vehicleOne = garage.get(0);
Vehicle vehicleTwo = garage.get(1);
```

We find ourselves knowing intuitively what type to declare `vehicleOne` and `vehicleTwo` as.
Since we are absolutely sure that garage is a list of a subtype of Vehicle, we know that
assigning its children to Vehicle will work.
It is in this sense that people say `garage` is a "producer" of sorts - it produces values,
and we consume from it, but we can never insert values into it. 

Applying this pattern into the context of a method would look something like this :

```java
List<Vehicle> factory = new ArrayList<>();
public void addAllToFactory(List<? extends Vehicle> vehicles){
    for(Vehicle oneVehicle : vehicles){
        factory.add(oneVehicle);
    }
}

List<Car> cars = new ArrayList<>();
cars.add(new Car());
addAllToFactory(cars); //works
```

The `cars` that we pass into `addAllToFactory` now takes on the role of a "producer",
yielding the values inside it for the `factory` to "consume". And since it a "producer", 
the vehicles parameter inside `addAllToFactory` uses the corresponding wildcard-extends (PE in PECS).

A similar logic applies for Consumer-Super.

```java
List<? super Car> garage = new ArrayList<>();
garage.add(new Car());        //runs
garage.add(new Roomba());     //runs
garage.add(new Vehicle());    // compilation error
```

The compiler knows that `garage` references a supertype of `Car`, but doesn't know which one.
I can add `Roomba` and `Car` to `garage` because they're all subtypes of `Car`, but `Vehicle`
needs to stay out. The type of `garage` may or may not be a parent of `Vehicle` - so in order to
avoid the possibility of a runtime error if garage turns out to be a subtype of `Vehicle`, the compiler
plays it safe and throws a compilation error.

Adding stuff to garage makes it take on the role of a **consumer**; what if we want it to **produce**?

```java
Car car = garage.get(0);         //error
Car car = (Car)garage.get(0);    //runs
Object object = garage.get(0);   //runs
```

Because the compiler doesn't know the exact type of garage, it only accepts the assignment of a returned value
to an `Object`, since all classes are Objects. If I want to assign it to something else, like `Car`, then I
need to be sure that the first element of garage if of type `Car` so I can downcast it. All these restrictions
makes wildcard-super a pretty poor producer. 

An example of wildcard-super doing what it does best - acting as a consumer:
```java
List<Car> cars = new ArrayList<>(List.of(new Car(), new Car()));
public void addCarsToFactory(List<? super Vehicle> factory){
    for(Car car : cars){
        factory.add(car);
    }
}

List<Vehicle> factory = new ArrayList<>();
addCarsToFactory(factory); //works
```

The factory parameter in `addCarsToFactory` is a consumer, so it uses super (CS of PECS). 
