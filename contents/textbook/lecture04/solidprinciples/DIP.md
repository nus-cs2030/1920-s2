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

For example, say we have a low-level LightBulb class:

```
public class LightBulb {
    public void turnOn() {
        System.out.println("LightBulb turned on!");
    }
    public void turnOff() {
        System.out.println("LightBulb turned off!");
    }
}
```

The LightBulb class has the methods turnOn() and turnOff() that turns a lightbulb on and off

Now, we have a high-level ElectricPowerSwitch class that is directly dependent on the low-level LightBulb class:

```
public class ElectricPowerSwitch {
    public LightBulb lightBulb;
    public boolean on;
    public ElectricPowerSwitch(LightBulb lightBulb) {
        this.lightBulb = lightBulb;
        this.on = false;
    }
    public boolean isOn() {
        return this.on;
    }
    public void press(){
        boolean checkOn = isOn();
        if (checkOn) {
            lightBulb.turnOff();
            this.on = false;
        } else {
            lightBulb.turnOn();
            this.on = true;
        }
    }
}
```

As seen in the code, the LightBulb class is "hardcoded" in the ElectricPowerSwitch class. This would then violate the Dependency Inversion Principle since a switch should not be tied to a lightbulb only. It should be able to function on other appliances and devices too, such as a fan or a refrigerator. This would mean great modifications to the ElectricPowerSwitch class if more appliances or devices class are added.

As such, to adhere to the principle, we will need an abstraction that the 2 classes can depend on, and this could be done by implementing interfaces.

```
public interface Switch {
    boolean isOn();
    void press();
}
```
```
public interface Switchable {
    void turnOn();
    void turnOff();
}
```

This way, any low-level switchable appliances or devices classes can implement the interfaces and provide their own functionality without worrying about modifying the ElectricPowerSwitch class.
