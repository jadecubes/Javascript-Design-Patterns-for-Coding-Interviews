# Builder Pattern
## What is the builder pattern?
The builder pattern is a type of creational pattern that helps in building complex objects using simpler objects. It provides a flexible and step-by-step approach towards making these objects. It also keeps the representation and process of creation shielded.

[Builder pattern example](./building.jpg)

Consider the example of a restaurant that has to build a meal for its customers. The building process has various stages:

- The customer comes in and places an orders for a meal to the cashier

- The cashier forwards the order to the crew member (the builder) who makes the order

- The builder assembles the meal consisting of the burger, fries, and soda of choice

- The cashier receives the final meal from the builder

- The customer receives the final order from the cashier

Every meal ordered by the customer regardless of the choices for the burger, fries, and soda will follow the same process. As you can see, a complex object is created following a step-by-step process.

## Example
Let’s look at a coding example that implements the builder pattern:

```javascript
function Meal() {
  this.make = function(builder){
    builder.step1();
    builder.step2();
    builder.step3();
    builder.step4();
    return builder.get();
  }
}
 
function MealBuilder(pattie,side,soda) {
   this.meal = null;
   this.step1 = function() {
        this.meal = new Order();
    };
 
    this.step2 = function() {
        this.meal.addBurger(pattie);
    };

    this.step3 = function(){
      this.meal.addSide(side);
    }
    
    this.step4 = function(){
      this.meal.addSoda(soda);
    }
 
    this.get = function() {
        return this.meal;
    };
}
 
function Order() {
    this.burger = null;
    this.side = null;
    this.soda = null; 
 
    this.addBurger = function(pattie) {
        this.burger = pattie;
    };
 
    this.addSide = function(side) {
        this.side = side;
    };

    this.addSoda = function(soda){
      this.soda = soda;
    }

    this.display = function(){
      console.log(`You meal has a ${this.burger} burger, ${this.side} on the side, and a ${this.soda}.`)
    }
}
 
var meal = new Meal();
var mealBuilder = new MealBuilder("chicken","curly fries","coke");
var chickenBurgerMeal = meal.make(mealBuilder);
chickenBurgerMeal.display();   
```

### Explanation
In this example, we are building a meal using a step-by-step process. We have a Meal and a builder object MealBuilder. The make function in Meal accepts a builder instance and builds the meal by going through these four steps:

- step1: creating a new meal

- step2: adding a burger to the meal

- step3: adding the side to the meal

- step4: adding a soda to the meal

At the end of these steps, the assembled meal (Order object) is returned in (line 7):
```javascript
return builder.get();
```
How are the burger, side, and soda added to the meal? The Order function contains the following methods which set the values of these properties:

- addBurger: sets the value of burger

- addSides: sets the value of sides

- addSoda: sets the value of soda

There is an additional method display, which shows the order that the customer placed. A customer can provide different builders, which gives him some control in the overall meal-building process. In the example above, the customer provided the following builder:
```javascript
var mealBuilder = new MealBuilder("chicken","curly fries","coke");
```
As you can see for different orders placed by the customers, the same builder pattern can be used.

## When to use the builder pattern?
You can use this design pattern when building apps that require you to create complex objects. It can help you hide the construction process of building these objects.

A good example would be a DOM, where you might need to create plenty of nodes and attributes. The construction process can get quite messy if you are building a complex DOM object. In cases like these, the builder pattern can be used.
