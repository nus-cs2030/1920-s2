<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Exception Handling
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture06/errorHandling/errorHandling.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

So you read about [Assertions](
https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture06/errorHandling/assertions.html)
and are now wondering, "what about those unexpected events when errors 
occur?" Good question. In this section, we will see how **exceptions** are
handled when they are thrown.

First of all, let's set the language right:
- **Exceptions** ("exceptional events") are errors or problems, which arise 
when a program is executed.
- **Throwing an exception** means raising the exception during a program
execution, such that the program stops immediately, or manually handled 
in a subsequent part of the code.
- **Catching**, or **handling an exception** is an action by which an
exception is "dealt with" to prevent the program from stopping abruptly.

When problems occur when we code, we can see them in our console. For 
example, you might have seen a `NullPointerException`, when you are trying
to perform an operation to a `null` object. There are two types of 
exceptions:
- **Checked exceptions**, thrown during compile time. They should be 
controlled as much as possible. 
- **Unchecked exceptions**, thrown during runtime. More often than not, they
are unexpected and can arise without the control of the developer. Sometimes,
a programmer can expect such exceptions to occur and make modifications to 
prevent this type of exceptions from arising.

Let's say we have this block of code:
```java
import java.util.Scanner;

public class TimesTwo {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println(sc.nextInt() * 2);
    }
}
```
Seems like a harmless little program to get the doubled value of an input
integer. We compile the code with `javac TimesTwo.java` and run it with
`java TimesTwo` and we input `2`--we get `4`. Seems reasonable.

Now what if a user is naughty and inputs `a` instead? Booyah, we see this
```
Exception in thread "main" java.util.InputMismatchException
        at java.base/java.util.Scanner.throwFor(Scanner.java:939)
        at java.base/java.util.Scanner.next(Scanner.java:1594)
        at java.base/java.util.Scanner.nextInt(Scanner.java:2258)
        at java.base/java.util.Scanner.nextInt(Scanner.java:2212)
        at TimesTwo.main(TimesTwo.java:6)
```
This is an example of an **unchecked exception**. The possibility of a user
inputting a non-integer value into our program is **uncontrollable**. In this
case, the name of the exception is `InputMismatchException`.

You may have seen in other programs, say Facebook, where you are required to
provide an email address, and if what you provided is not an email address, the
website nicely tells you to check and give a correct email address, instead of
throwing an exception. This is exactly **error handling** (maybe not exactly,
but you get the idea).

If we suspect a code may give an exception, we can encapsulate them in a 
**try-catch clause** like so:
```java
import java.util.Scanner;
import java.util.InputMismatchException;

public class TimesTwo {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        try {
            System.out.println(sc.nextInt() * 2);
        } catch (InputMismatchException e) {
            System.out.println("Please give an integer!");
        }
    }
}
```
Then, when a user inputs a non-integer, `InputMismatchException` will be 
invoked and the `Please give an integer!` error will be displayed. If we want
to have the user try again, we can wrap the operation in a function like so:
```java
import java.util.Scanner;
import java.util.InputMismatchException;

public class TimesTwo {
    public static void do() {
        Scanner sc = new Scanner(System.in);
        try {
            System.out.println(sc.nextInt() * 2);
        } catch (InputMismatchException e) {
            System.out.println("Please give an integer!");
            TimesTwo.do();
        }
    }
    public static void main(String[] args) {
        TimesTwo.do();
    }
}
```

Note that in this case, we only handle `InputMismatchException`. Other types 
of exception can also arise, and they will be displayed if we don't handle them.
Consider an empty file `empty.in`. Then if we invoke
```
java TimesTwo < empty.in
```
we will (unsurprisingly) get a `NoSuchElementException`! We may wish to handle it:
```java
import java.util.Scanner;
import java.util.InputMismatchException;
import java.util.NoSuchElementException;

public class TimesTwo {
    public static void do() {
        Scanner sc = new Scanner(System.in);
        try {
            System.out.println(sc.nextInt() * 2);
        } catch (InputMismatchException e) {
            System.out.println("Please give an integer!");
            TimesTwo.do();
        } catch (NoSuchElementException e) {
            System.out.println("Please give valid input file!");
            // why is there no TimesTwo.do() here?
        }
    }
    public static void main(String[] args) {
        TimesTwo.do();
    }
}
```
You may also wish to think about what happens if there is no input at all; can
we get a `NullPointerException`? Can we have no input at all?

What if, we have a program which reads and creates a set of data, then finally 
saves it into a file.
```java
import com.purfectliterature.Data;
import com.purfectliterature.DataSet;
import com.purfectliterature.Image;
import com.purfectliterature.Internet;
import com.purfectliterature.Device;
import com.purfectliterature.File;

public class Steal {
    private static final String SECRET_LINK = "https://secret.apple.com/iphone";

    public static void main(String[] args) {
        DataSet dataSet = new DataSet(Internet.download(Steal.SECRET_LINK);
        Device[] upcomingIphones = new Device[dataSet.size()];
        
        int i = 0;
        while (dataSet.size() > 0) {
            Data iphone = dataSet.poll();
            Image img = Internet.download(Steal.SECRET_LINK + "/" + iphone.getName() + "/image.png");
            upcomingIphones[i++] = new Device(iphone.getName(), img);
        }
        
        File.write(upcomingIphones.toCsv(), "upcoming_iphones.csv");
        System.out.println("Stealing secret info from Apple completed.");
    }
}
```
You can imagine that we have this imaginary program which will go and download
all the upcoming iPhones from an imaginary Apple secret website, then process the
big downloaded file, download their respective images, and store it in an array
of `Device`s. Finally, the program writes all the retrieved information to a .csv
file.

You may wish to expect something may happen when we try to `Internet.download` 
something from the internet (e.g. thunderstorm, cable unplugged, never pay bill).
For the first `Internet.download(Steal.SECRET_LINK)`, if this terminates the 
program, it is okay. But, if an error occurs at 
```java
Image img = Internet.download(Steal.SECRET_LINK + "/" + iphone.getName() + "/image.png");
```
and the program terminates, all our hard work and stored information will be gone 
to garbage collection, since our program terminates. So, you might want to 
encapsulate the error in a try-catch clause. But, this is not enough. You want to be
able to show an error message, but then you need an assurance that whatever happens,
you will at least write whatever you have collected so far into a .csv file. Enter
`finally`.

We can use `finally` to specify what we want to **always** be evaluated after the end
of a try-catch clause. So, we have
```java
...
        while (dataSet.size() > 0) {
            Data iphone = dataSet.poll();
            try {
                Image img = Internet.download(Steal.SECRET_LINK + "/" + iphone.getName() + "/image.png");
            } catch (Internet.NoInternetError e) {
                System.out.println(e);
            } finally {
                try {
                    File.write(upcomingIphones.toCsv(), "upcoming_iphones.csv");
                } catch (File.FileNotFoundError e) {
                    String newFileName = new Scanner(System.in).nextLine();
                    File.write(upcomingIphones.toCsv(), newFileName + ".csv");
                }
            }
            upcomingIphones[i++] = new Device(iphone.getName(), img);
        }
...
```
We even considered the case if we cannot write the file, and if so, we prompt the user
to give us an appropriate name to save the file. How cool is that?

Maybe, just maybe, there could be some other errors that **we don't know** and we really
don't want the code to terminate from an error. We can handle `Exception`.

Every exceptions we see in Java extends `Exception`, and `Exception` extends `Throwable`.
We can handle `Exception` to handle just about **any** errors in Java.
```java
import java.util.Scanner;
import java.util.InputMismatchException;
import java.util.NoSuchElementException;

public class TimesTwo {
    public static void do() {
        Scanner sc = new Scanner(System.in);
        try {
            System.out.println(sc.nextInt() * 2);
        } catch (InputMismatchException e) {
            System.out.println("Please give an integer!");
            TimesTwo.do();
        } catch (NoSuchElementException e) {
            System.out.println("Please give valid input file!");
            // why is there no TimesTwo.do() here?
        } catch (Exception e) {
            System.out.println("Something happened:");
            System.out.println(e);
        } finally {
            System.out.println("Eating clock very is time-consuming.");
        }
    }
    public static void main(String[] args) {
        TimesTwo.do();
    }
}
```
Note that the **order** of exception handling matters. Imagine what will happen if we
handle `Exception` first:
```java
import java.util.Scanner;
import java.util.InputMismatchException;
import java.util.NoSuchElementException;

public class TimesTwo {
    public static void do() {
        Scanner sc = new Scanner(System.in);
        try {
            System.out.println(sc.nextInt() * 2);
        } catch (Exception e) { // Exception is now first to be handled!
            System.out.println("Something happened:");
            System.out.println(e);
        } catch (InputMismatchException e) {
            System.out.println("Please give an integer!");
            TimesTwo.do();
        } catch (NoSuchElementException e) {
            System.out.println("Please give valid input file!");
            // why is there no TimesTwo.do() here?
        } finally {
            System.out.println("Eating clock very is time-consuming.");
        }
    }
    public static void main(String[] args) {
        TimesTwo.do();
    }
}
```
then, the other handles for `InputMismatchException` and `NoSuchElementException`
will **never** be evaluated, because every exception extends `Exception`.
