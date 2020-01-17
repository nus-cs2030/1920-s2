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
        
        System.out.println(numOfExpiredFoods);
    }
}
```
