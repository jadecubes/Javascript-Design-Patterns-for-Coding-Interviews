# Decorator Pattern
## What is the decorator pattern?
The decorator pattern focuses on adding properties, functionalities, and behavior to existing classes dynamically. The additional decoration functionalities aren’t considered essential enough to be a part of the original class definition as they can cause clutter. Hence, the decorator pattern lets you modify the code without changing the original class.

Unlike the creational patterns, the decorator pattern is a structural pattern that does not focus on object creation rather decoration. Hence, it doesn’t rely on prototypal inheritance alone; it takes the object and keeps adding decoration to it. This makes the process more streamlined. Let’s take a look at an example to understand this concept better.

## Example
```javascript
class FrozenYoghurt {
  constructor(flavor, price) {
    this.flavor = flavor
    this.price = price
  }

  orderPlaced() {
    console.log(`The ${this.flavor} flavor will cost you ${this.price} dollars`);
  }
}

// decorator 1
function addFlavors(froyo) {
  froyo.addStrawberry = true;
  froyo.addVanilla = true;
  froyo.price += 20;
  froyo.updatedInfo = function(){
    console.log(`The updated price after adding flavors is ${froyo.price} dollars`)
  }
  return froyo;
}

// decorator 2
function addToppings(froyo) {
  froyo.hasSprinkles = true;
  froyo.hasBrownie =  true;
  froyo.hasWafers = true;
  froyo.allToppings = function(){
    console.log("Your froyo has sprinkles, brownie, and wafers")
  }
  return froyo;
}

//using decorators
//creating a froyo
const froyo = new FrozenYoghurt("chocolate",10)
froyo.orderPlaced()
//adding flavors
var froyowithFlavors = addFlavors(froyo)
froyowithFlavors.updatedInfo()
//adding toppings
var froyoWithToppings = addToppings(froyo)
froyoWithToppings.allToppings()
```

### Explanation
Let’s start by visualizing the code:

[Decorator pattern example](./deco.jpg)


From the diagram above, you can see that we first create a chocolate froyo that costs 10 dollars. Anyone who wants a froyo will perform this step.

After the first step, you have the option to add more flavors. Some people might get a single flavor, pay for it, and then leave. However, in our case, we add two more flavors to it, strawberry and vanilla. Each of them costs 10 dollars, so now, we will have to pay an extra 20 dollars.

At this point, you have the option to either pay and leave or add extra toppings to the froyo. In our case, we add three toppings: sprinkles, a brownie, and wafers.

So, how is the decorator pattern being implemented here? Getting the chocolate froyo was the major step. However, the additional flavors and the toppings are both optional. Hence, they are both additional choices that the person may or may not consider. So, both of these steps are not included in the first step, instead, they’re added later on as decoration.

Now that the concept is clear, let’s look at its implementation using code.

We start by creating a FrozenYoghurt class like so:
```javascript
class FrozenYoghurt {
  constructor(flavor, price) {
    this.flavor = flavor
    this.price = price
  }

  orderPlaced() {
    console.log(`The ＄{this.flavor} flavor will cost you ＄{this.price} dollars`);
  }
}
```
Its constructor initializes the flavor and price properties to the values passed into the constructor. It also contains the orderPlaced function, which displays the flavor you got and its price.

Now, it’s time for the decoration. You have the option to addFlavors and addToppings to your froyo.

Here’s what the addFlavors function looks like:
```javascript
function addFlavors(froyo) {
  froyo.addStrawberry = true;
  froyo.addVanilla = true;
  froyo.price += 20;
  froyo.updatedInfo = function(){
    console.log(`The updated price after adding flavors is ＄{froyo.price} dollars`)
  }
  return froyo;
}
```
It adds the strawberry and vanilla flavors to the froyo instance and increases the price by 20 dollars. The updatedInfo function shows the updated price.

The addToppings function is as follows:
```javascript
function addToppings(froyo) {
  froyo.hasSprinkles = true;
  froyo.hasBrownie =  true;
  froyo.hasWafers = true;
  froyo.allToppings = function(){
    console.log("Your froyo has sprinkles, brownie, and wafers")
  }
  return froyo;
}
```
It adds sprinkles, a brownie, and wafers to the froyo instance. The allToppings function displays all the toppings added.

## When to use the decorator pattern?
JavaScript developers can use the decorator pattern when they want to easily modify or extend the functionality of an object without changing its base code.

It can also be used if an application has a lot of distinct objects with the same underlying code. Instead of creating all of them using different subclasses, additional functionalities can be added to the objects using the decorator pattern.

A simple example is text formatting, where you need to apply different formattings such as bold, italics, and underline to the same text.
