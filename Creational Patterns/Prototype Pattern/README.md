# Prototype Pattern
## What is the prototype pattern?
The prototype creational pattern is used to instantiate objects with some default values using an existing object. It clones the object and provides the existing properties to the cloned object using prototypal inheritance.

In prototypal inheritance, a prototype object acts as a blueprint from which other objects inherit when the constructor instantiates them. Hence, any properties defined on the prototype of a constructor function will also be present in the cloned object it creates.

[Prototype pattern example]

In the illustration above, you can see that a prototype for the car (a blueprint) is present from which the other car objects, car 1 and car 2, are created. So, how are these created? In JavaScript, objects can be cloned using the Object.create method. Let’s look at it in detail in the example below.


## Example
```javascript
var car = {
    drive(){
        console.log("Started Driving")
        },
    brake(){
        console.log("Stopping the car")
    },
    numofWheels : 4  
} 

const car1 = Object.create(car);
car1.drive();
car1.brake();
console.log(car1.numofWheels);

const car2 = Object.create(car)
car2.drive();
car2.brake();
console.log(car2.numofWheels);
```
### Explanation
From the code above, you can see that both car1 and car2 have the properties and methods present in the car object. What does Object.create do? Object.create takes an object as a parameter (car in our case) and returns an object whose prototype property points to this object (car).

```javascript
var car = {
    drive(){
        console.log("Started Driving")
        },
    brake(){
        console.log("Stopping the car")
    },
    numofWheels : 4  
} 

const car1 = Object.create(car);
console.log((car1.__proto__) == car) 

const car2 = Object.create(car)
```

As you can see, the prototype property of both car1 and car2 accessed using __proto__ is car.

You can also define an extra property for the new object using Object.create. Let’s add additional properties to car1 and car2.

```javascript
var car = {
    drive(){
        console.log("Started Driving")
        },
    brake(){
        console.log("Stopping the car")
    },
    numofWheels : 4  
} 

//defining the extra property color with value red
const car1 = Object.create(car,{color :{value: "red"}});
console.log(car1.color)

//defining the extra property color with value red black
const car2 = Object.create(car, {color : {value: "red black"}});
console.log(car2.color)
```

In the example, we added the color property to both car1 and car2 with their value set to red and red black respectively.

## When to use the prototype pattern?
The prototypal pattern has native support in JavaScript. It involves cloning an already-configured object. Hence, the cloned objects are created by reference instead of having their own separate copies. This boosts the performance and efficiency of code. This pattern can also be used in the following cases:

- To eliminate the overhead of initializing an object

- When you want the system to be independent about how the products in it are created

- When creating objects from a database, whose values are copied to the cloned object
