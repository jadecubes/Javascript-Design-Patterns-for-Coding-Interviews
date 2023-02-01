# Constructor Pattern

## What is the constructor pattern?
As a JavaScript developer, you may have encountered “constructors” at some point. These are special functions that initialize objects with specific properties and methods.

The “constructor pattern”, as the name defines, is a class-based pattern that uses the constructors present in the class to create specific types of objects.


## Example
There are many ways to create objects in JavaScript, such as using the {} notation or using Object.create.

However, in JavaScript, the use of the “constructor” pattern is very popular as it can create multiple objects of a specific kind. Let’s take a look at an example:

```javascript
function Human(name, age, occupation){ //ES5 function based constructor 
  //defining properties inside the constructor function
  //constructor initializing the property values upon object creation
  this.name = name; 
  this.age = age;
  this.occupation = occupation;
  //defining a method inside the constructor function
  this.describe = function(){
    console.log(`${this.name} is a ${this.age}-year-old ${this.occupation}`);
  }
}
//creating a "person" object using the Human constructor
//the constructor uses the arguments passed into it to
//initialize the property values name, age and sex
var person = new Human("Elle", "23", "Engineer");
//calling the describe method for the person object
person.describe();
```


### Explanation

The ES5 version of JavaScript uses functions as constructors. These functions are used to instantiate objects. In the example above, the constructor function Human is defined. It has the following properties and method:

- age

- name

- occupation

- describe()

In line 15, an object is created from Human using the new keyword. It will contain all the properties and methods defined in Human.

```javascript
var person = new Human("Elle", "23", "Engineer"); //line 15
```

[Creating an object using constructor function]


When the person object is created, it will have its name, age, and occupation properties set to the arguments passed to the constructor function.

Similarly, it will also contain the method describe. In line 17, it is invoked for the person object

```javascript
person.describe();
```
and the following output is seen on the console:
```
"Elle is a 23-year-old Engineer"
```
How was this output achieved? You might have noticed the use of the this keyword inside the Human constructor function. this refers to the new object that is being created, person, in our case. Hence, in the lines

```javascript
this.name = name; 
this.age = age;
this.occupation = occupation;
```

this refers to the person object. Similarly, in the line

```
 console.log(`＄{this.name} is a ＄{this.age}-year-old ＄{this.occupation}`);
```

this refers to the person object, hence, displaying the output above.

We have discussed being able to create multiple objects using the constructor as a template. Now, let’s do that for the example above:

```javascript
function Human(name, age, occupation){ 
  this.name = name; 
  this.age = age;
  this.occupation = occupation;
  this.describe = function(){
    console.log(`${this.name} is a ${this.age}-year-old ${this.occupation}`);
  }
}

var person = new Human("Elle", "23", "Engineer");
person.describe();
//creating a second object
var newperson = new Human("Joe", "13","Painter");
newperson.describe();
```

Now we have a second object, newperson. As explained above, this refers to the new object created. Hence, line 13 will set the properties of the newperson object equal to the arguments passed in the constructor. Similarly, newperson.describe() will show the following as output:

```
"Joe is a 13-year-old Painter"
```

[Creating multiple objects using a constructor function]


## When to use the constructor pattern?

You can use it when you want to create multiple instances of the same template, since the instances can share methods but can still be different. Some examples of where it can be useful include:

- libraries

- plugins
