Imagine that we want to create a new class that encapsulates a queue of customers that visits a particular store. 
We will write :

```
class CustomerQueue {
  private Customer[] customers;
   :
  public CustomerQueue(int size) {...}
  public boolean isFull() {...}
  public boolean isEmpty() {...}
  public void insert(Customer c) {...}
  public Customer remove() {...}
}
```

Let say each customer travels using their own car when visiting the store.
We might then need a new class that encapsulates a queue of cars to keep track of the parking record.
We will write:

```
class CarQueue {
  private Car[] cars;
   :
  public CarQueue(int size) {...}
  public boolean isFull() {...}
  public boolean isEmpty() {...}
  public void insert(Car c) {...}
  public Car remove() {...}
}
```

We will then realise that there are actually many repetition in our code. By referring back to the _abstraction principle_, which states that "_Where similar functions are carried out by distinct pieces of code, it is generally beneficial to combine them into one by abstracting out the varying parts._", we should instead create a more generalised queue of Objects to replace the two class above.

```
class ObjectQueue {
  private Object[] objects;
   :
  public ObjectQueue(int size) {...}
  public boolean isFull() {...}
  public boolean isEmpty() {...}
  public void insert(Object c) {...}
  public Object remove() {...}
}
```
With this, we have a generalised class that is able to store objects of any kind, including its subclass such as a queue of strings, a queue of integers, etc.

Lets say the Customer and Car classes are :

```
class Customer {
 private Car carOwned;
 Customer(Car car) {
  carOwned = car;
 }
 :
}
```

```
class Car {
 private String licensePlate;
 Car(String string) {
  licensePlate = string;
 }
}
```

To create a queue of 5 customers and further add more customers, we can do : 

```
ObjectQueue cq = new ObjectQueue(5);
cq.insert(new Customer(new Car("ABC"));
cq.insert(new Customer(new Car("CDE"));
 : 
```

We can get the customer out of the queue by :

`Customer c = cq.remove();`

However, we will get a compilation error, since we are trying to perform a narrowing reference conversion. We cannot assign a variable of type `Object` to a variable of type `Customer` without type casting :

`Customer c = (Customer)cq.remove();`

This would work if the queue solely consists of only Customer. However, the code above might result in a runtime `ClassCastException` if there is an object in the queue that is not `Customer` or it's subclass. In order to avoid this, we can check the type of the object first:

```
Object o = cq.remove();
if (o instanceof Customer) {
 Customer c = (Customer) o;
}
```

However, this is troublesome as we need to check for the type of the object all the time in order to ensure that there is no complication error.  
Luckily for us Java 5 introduces generics and it is able to solve this issue. By allows a generic class or generic interface of some type T to be written:

```
class Queue<T> {
  private T[] objects;
   :
  public Queue(int size) {...}
  public boolean isFull() {...}
  public boolean isEmpty() {...}
  public void insert(T o) {...}
  public T remove() {...}
}
```

`T` is known as type parameter. 
The same code previously can be rewritten as :

```
Queue<Customer> cq = new Queue<Customer>(5);
cq.insert(new Customer(new Car("ABC"));
cq.insert(new Customer(new Car("CDE"));
 : 
Customer c = cq.remove();
```
In the code above, we set the type parameter `T` as `Customer`, creating a _parameterised type_ `Queue<Customer>`.
In Line 4, we no longer need to type cast, because there is no longer any danger of running into runtime error due to the possibility of an object of the wrong class added to the queue.
This is because:

```
Queue<Customer> cq = new Queue<Customer>(3);
cq.insert(new Car("ABC"));
```

This will generate a complie-time error.
Hence, we are able to enable the complier to produce an error if we try to insert a non-`Customer` into our queue of `Customer` objects. 

In conclusion: Generic typing is a type of polymorphism and it is also known as parametric polymorphism.
