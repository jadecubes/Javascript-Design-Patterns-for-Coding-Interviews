# Abstract Pattern

## What is the abstract pattern?
To understand the abstract pattern, let’s go back to the factory pattern. We use the factory pattern to create multiple objects from the same family without having to deal with the creation process. The abstract pattern is similar; the difference is that it provides a constructor to create families of related objects. It is abstract, meaning it does not specify concrete classes or constructors.

[Abstract pattern example](./abstract.jpg)

The diagram above should help you understand the concept better. Here, there is an “Abstract Junk Food Factory”. It provides a constructor to create instances of either the “Chips Factory” or the “Soda Factory”. As you can see, both factories are different, yet related. These factories then, in turn, produce different types of products, chips, and sodas.

## Example
Let’s look at a sample code snippet for the abstract pattern below:

```javascript
function Soda(name,type,price) {
    this.name = name;
    this.type = type;
    this.price = price;
    this.display = function(){
        console.log(`The ${this.type} ${this.name} costs ${this.price} dollars`)
    }
}

function Chips(name,type,price) {
    this.name = name;
    this.type = type;
    this.price = price;
    this.display = function(){
        console.log(`The ${this.type} ${this.name} costs ${this.price} dollars`)
    }
}

function JunkFoodFactory(){
    var junkfood;
    this.createJunkFood = function(name,type,price) {
        switch (name) {
            case "chips":
                junkfood =  new Chips(name,type,price);
                break;
            case "soda":
                junkfood = new Soda(name,type,price);
                break;
            default:
                junkfood = new Chips(name,type,price);
                break;
        }
        return junkfood;
    }  
}
 

var factory = new JunkFoodFactory();
var chips = factory.createJunkFood("chips","potato",1.50)
chips.display()

chips = factory.createJunkFood("chips","corn",2.50)
chips.display()

var soda = factory.createJunkFood("soda", "Energy Drink", 10)
soda.display()

soda = factory.createJunkFood("soda", "Cola", 7)
soda.display()
```



### Explanation
In the example above, we have an abstract factory JunkFoodFactory. Let’s discuss it step by step.

The constructor function JunkFoodFactory contains the method createJunkFoodFactory.
```javascript
 this.createJunkFoodFactory = function(name,type,price) {//code...}
 ```
It accepts the following parameters: name, type, and price.

The function contains the variable junkfood used to store an instance of junk food, be it chips or soda.

If the name argument is chips, the Chips factory constructor is invoked with the required parameters to create a pack of chips.
```javascript
junkfood =  new Chips(name,type,price);
```
If the name argument is soda, the Soda factory constructor is invoked with the required parameters to create a can of soda.
```javascript
junkfood =  new Soda(name,type,price);
```
In the end, the junkfood instance is returned.

Now, let’s look at the Chips and Soda constructors.

Both the Chips and Soda constructors accept the parameters name, type, and price. The properties of an instance will get initialized to the arguments passed.
```javascript
function Soda(name,type,price) {
  this.name = name;
  this.type = type;
  this.price = price;
}

function Chips(name,type,price) {
  this.name = name;
  this.type = type;
  this.price = price;
}
```
Both constructors also contain the display function. It shows the name, type as well as the price of the instance created.
```javascript
this.display = function(){
      console.log(`The ＄{this.type} ＄{this.name} costs ＄{this.price} dollars`)
}
```
In the end, all you have to do is create a factory, invoke the createJunkFood function on it (with the required arguments), and the rest is taken care of. You don’t have to create new instances for chips or sodas using the new operator yourself. You can simply call the createJunkFood function that will decide which factory to invoke to create the required product.

## When to use the abstract pattern?
The abstract pattern for creating instances is preferred over initializing when using the new operator since constructors have limited control over the process. Whereas, a factory (pattern?) will have broader knowledge.

The use cases for this pattern are:

- applications requiring the reuse or sharing of objects

- applications with complex logic because they have multiple families of related objects that need to be used together

- object caching

- when the object creation process is to be shielded from the client
