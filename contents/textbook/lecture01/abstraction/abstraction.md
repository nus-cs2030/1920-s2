<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Abstraction
<hr>

### 

According to the Oxford Dictionary, the definition of the word abstraction is "the process of considering something independently of its associations or attributes." 

What can we learn from this and apply it to computer science? Well, as we have found out, abstraction is the ability to think about something without the details of its specific features. 

For example, let’s say that you are the owner of a soft drink factory, and you produce soft drinks of the following types: Coke, Pepsi, Sprite, Fanta, and Ribena.

These soft drinks generally have the following characteristics:

Type of container
Volume of container
Colour of drink
IsCarbonated

As the consumers of soft drinks, it makes it easier knowing that all soft drinks have the same set of features, making it easier to make a purchase decision based on my needs. 

Additionally, as the factory owner, it is also more convenient for you, since every drink has the same set of features. You probably also meed a couple more details about it, such as:

ExportDestinations
Manufacturing cost
ProfitPerUnit

Hence, we end up with a class that looks like this:
 
```
class SoftDrink {
    String typeOfContainer;
    int volumeOfContainer;
    String colour;
    boolean isCarbonated;
    //The information below I do not want to reveal to the public
    private int numberExportCountries = 16;
    private double manufactureCost = 0.5;
    private double profitPerUnit = 0.1;
    // constructor
    
    SoftDrink(String type, int vol, String col, boolean carb) {
        typeOfContainer = type;
        volumeOfContainer = vol;
        colour = col;
        isCarbonated = carb;
    }
}
```

Abstractions gives us a template that allow us to simply punch in the details of the object that I want to create, instead of creating something entirely different. For example, I want to create the Coke drink:
```
    public static void main(String[] args) {
        SoftDrink Coke = new SoftDrink("Can", 330, "Red", true);
        System.out.println(Coke.colour);
    }
```

Now I have created a Coke object, whose information that I inserted and made available can be accessed easily. 

But what about the other information that I do not want to reveal?

Firstly, they are given the access modifier `private`, which means that other classes are not able to access its information.
But more importantly, we do not use these information in the constructor. Hence I am able to exclude the use of these information 
when I construct the object.

But wait, is creating a soft drink really that simple? What about all the ingredients that go into the manufacturing process? Aren't those important too? Our constructor only accepts 4 arguments. 

The constructor is also another important aspect of abstraction. It is like a black box. It takes in the necessary information, the information I want to specify, and ignores all other aspects and details, but produces the object anyway. This way, you can afford to just focus on the details that really matter.

If you are being overloaded with information, which a soft drink factory owner(who also writes programmes!) definitely is, abstraction is one of the most powerful tools in his box.

**END**

##
Thank you for reading! Please drop a comment if you have questions or to give feedback.

<!-- DO NOT DELETE THIS LINK --> 
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture01/abstraction/abstraction.md)
<!-- DO NOT DELETE THIS LINK --> 
