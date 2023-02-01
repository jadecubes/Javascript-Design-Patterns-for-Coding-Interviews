# Singleton Pattern

## What is the singleton pattern?
The singleton pattern is a type of creational pattern that restricts the instantiation of a class to a single object. This allows the class to create an instance of the class the first time it is instantiated. However, on the next try, the existing instance of the class is returned. No new instance is created.

[Singleton](./singleton)

## Example
A real-life example is a printer a couple of office employees want to use. It’ll be a shared resource amongst all the employees. Hence, a single instance of the printer is required so that everyone can share instead of having a new instance for each employee who wants to print something.

[Singleton pattern example](./case.jpg)

Let’s see how we can implement the singleton pattern for the printer:


```javascript
let instance = null;
class Printer {
  constructor(pages) {
    this.display= function(){
      console.log(`You are connected to the printer. You want to print ${pages} pages.`)
    }
  }

  static getInstance(numOfpages){
    if(!instance){
      instance = new Printer(numOfpages);
    }
    return instance;
  }
}

var obj1 = Printer.getInstance(2)
console.log(obj1)
obj1.display()
var obj2 = Printer.getInstance(3)
console.log(obj2)
obj2.display()
console.log(obj2 == obj1)
```

### Explanation
In the example above, we are implementing the singleton pattern. The class Printer can only have a single instance. We ensure this in the getInstance function. Let’s look at it in detail.

The getInstance function accepts the parameter numOfpages. Inside the function, in line 10, we check if an instance for the Printer class already exists.
```javascript
if(!instance) //line 10
```
If it does not exist, the code inside the if condition executes and a new instance is created.

```javascript
instance = new Printer(numOfpages)
```
However, if an instance already exists, it simply returns the existing instance instead of creating a new one.
```javascript
return instance
```
You can see this in the output as well. We create an instance of the Printer with 2 passed as the argument to getInstance:
```javascript
var obj1 = Printer.getInstance(2)
console.log(obj1) //line 18
obj1.display() //line 19
For lines 18 & 19, you see the following output on the console:
```

```
"Printer { display: [Function] }"
"You are connected to the printer. You want to print 2 pages."
Next, we try to create a second instance of the Printer with 3 passed as the argument to getInstance.
```

```javascript
var obj2 = Printer.getInstance(3)
console.log(obj2) //line 21
obj2.display() //line 22
```
You can see the following output for the commands on lines 21 & 22:
```
"Printer { display: [Function] }"
"You are connected to the printer. You want to print 2 pages."
```
As you can see, instead of a new instance, the same instance is returned. Hence, obj2.display shows 2 pages instead of 3 pages. Line 23 also returns true, showing that both the instances are the same.
```javascript
console.log(obj2 == obj1) //line 23
```
## When to use the singleton pattern?
The singleton pattern is mostly used in cases where you want a single object to coordinate actions across a system. Singletons are mostly used by:

- Services: services can be singleton since they store the state, configuration, and provide access to resources. Therefore, it makes sense to have a single instance of a service in an application.

- Databases: when it comes to database connections, databases such as MongoDB utilize the singleton pattern.

- Configurations: if there is an object with a specific configuration, you don’t need a new instance every time that configuration object is needed.
