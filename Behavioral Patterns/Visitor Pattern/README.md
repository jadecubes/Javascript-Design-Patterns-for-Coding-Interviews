# Visitor Pattern
## What is the visitor pattern?
The visitor pattern allows the definition of new operations to the collection of objects without changing the structure of the objects themselves. This allows us to separate the class from the logic it implements.

The extra operations can be encapsulated in a visitor object. The objects can have a visit method that accepts the visitor object. The visitor can then make the required changes and perform the operations on the object that received it. This allows the developers to make future extensions, extend the libraries/frameworks, etc.

[Visitor pattern concept](./concept.jpg)

## Example
```javascript
class Visitor {
    visit(item){}
}

class BookVisitor extends Visitor {
    visit(book) {
        var cost=0; 
        if(book.getPrice() > 50) 
        { 
            cost = book.getPrice()*0.50 
        } 
        else{
            cost = book.getPrice()
        }     
        console.log("Book name: "+ book.getName() + "\n" + "ID: " + book.getID() + "\n" + "cost: "+ cost); 
        return cost; 
    }
}

class Book{
    constructor(id,name,price){
        this.id = id
        this.name = name
        this.price = price
    }
    getPrice(){
        return this.price
    }
    getName(){
        return this.name
    }
    getID(){
        return this.id
    }
    accept(visitor){
        return visitor.visit(this)
    }
}

var visitor = new BookVisitor()
var book1 = new Book("#1234","lordOftheRings",80)
book1.accept(visitor)
```
## Explanation
In the example above, we have a bookshop. The Book class is used to represent a book in the shop. It is defined as follows:
```javascript
class Book{
    constructor(id,name,price){
        this.id = id
        this.name = name
        this.price = price
    }
    //code...
}
```
A Book has the following properties:

- An id

- A name

- A price

It also contains the following functions:
```javascript
getPrice(){
    return this.price
}

getName(){
    return this.name
}

getID(){
    return this.id
}
```
The getPrice method returns the price, getName returns the name, and getID returns the id of the book.

Now, the bookshop introduces a discount on the books that cost more than 50 dollars. Hence, we want to perform an additional operation of visiting the books and implementing the discount on them. Here, we use the visitor pattern. We introduce a visitor that will visit the books and update their prices. However, the book objects should have a function that allows the visitor to visit them and perform the operation. For this purpose, we have defined the accept method in our Book class:
```javascript
accept(visitor){
    return visitor.visit(this)
}
```
accept takes a visitor object as an argument and allows it to visit the current book (this is pointing to the current book) by calling its visit function.

Now, let’s look at our Visitor class:
```javascript
class Visitor {
   visit(item){}
}
```
Visitor has a visit function that takes the item it wants to visit as a parameter. In our case, we want to visit the books. Therefore, we first define the BookVisitor class (extends the Visitor class) and then its visit function:
```javascript
class BookVisitor extends Visitor {
   visit(book) {
      var cost=0; 
      if(book.getPrice() > 50) 
      { 
         cost = book.getPrice()*0.50 
      } 
      else{
         cost = book.getPrice()
      }     
      console.log("Book name: "+ book.getName() + "\n" + "ID: " + book.getID() + "\n" + "cost: "+ cost); 
      return cost; 
   }
}
```
The visit function checks the price of the book it is visiting. If it is greater than 50, it applies a fifty percent discount to it, else, it keeps the price as it is.

## When to use the visitor pattern?
Visitor pattern can be used when:

- Similar operations need to be performed on different objects of a data structure

- Specific operations need to be performed on different objects in the data structure

- You want to add extensibility to libraries or frameworks
