# Mediator Pattern
## What is the mediator pattern?
It is a behavioral pattern that allows a mediator (a central authority) to act as the coordinator between different objects, instead of the objects referring to each other directly. A mediator as the name implies, is a central authority through which various components can communicate. It allows the loose coupling of objects.

A real-life example is a chat application. Here, the chat box acts as the mediator through which various users interact with one another.

[Chat Room]

### Example

```javascript
class User{
  constructor(name,userId){
    this.name = name
    this.userId = userId
    this.chatbox = null;
  }
  sendMessage(message,sendTo){
    this.chatbox.send(message,this,sendTo)
  }
  receiveMessage(message,receiveFrom){
    console.log(`${receiveFrom.name} sent the message: ${message}`)
  }
}

class ChatBox{
  
  constructor(){
    this.users = []
  }
  
  register(user){
    this.users[user.userId] = user
    user.chatbox = this;
  }

  send(message,receiveFrom,sendTo){
      sendTo.receiveMessage(message, receiveFrom);    
  }
}

var chatbox = new ChatBox();
var joey = new User("Joey",1);
var phoebe = new User("Phoebe",2);
chatbox.register(joey);
chatbox.register(phoebe);
joey.sendMessage("Hey, how you doing?",phoebe);
phoebe.sendMessage("Smelly Cat, Smelly Cat..",joey);
joey.sendMessage("I love this song!", phoebe);
```
## Explanation
The code above implements a chat box. Different users access the chat box to interact with each other. Let’s start by discussing the User class first.
```javascript
class User{
  constructor(name,userId){
    this.name = name
    this.userId = userId
    this.chatbox = null;
  }
  //code...
}
```
A User has the following properties:

name: the name of the user

userId: a unique id assigned to the user to keep track

chatbox: the chatbox instance the user is sending/receiving messages from

Every User has the option to send a message and to receive one.

Let’s look at the sendMessage function first:
```javascript
sendMessage(message,sendTo){
   this.chatbox.send(message,this,sendTo)
}
```
To send a message, the User specifies what the message is (first parameter of the sendMessage function) and to whom they want to send the message (the second parameter of the sendMessage function). The chatbox, which acts as the mediator, invokes its send function to send the message from the user (this) to the recipient specified (sendTo).

Now, let’s look at the receiveMessage function:
```javascript
receiveMessage(message,receiveFrom){
   console.log(`＄{receiveFrom.name} sent the message: ＄{message}`)
}
```
The receiveMessage function displays the message (first parameter of the receiveMessage function) and the name of the user (second parameter of the receiveMessage function) from whom they received the message.

Now, let’s understand the Chatbox class.
```javascript
class ChatBox{
  
  constructor(){
    this.users = []
  }
  //code...
}
```
The constructor initializes the users array. It stores all the users that register with the chat box. Here’s what the register function looks like:
```javascript
register(user){
    this.users[user.userId] = user
    user.chatbox = this;
}
```
Whenever a new user is registered, they are added to the users array and their chatbox property is set to this, that is, they become a part of the chatbox.

The purpose of the chatbox is to receive a message from one user and send it to the other. Here’s what its send function looks like:
```javascript
send(message,receiveFrom,sendTo){
   sendTo.receiveMessage(message, receiveFrom);    
}
```
It takes the message received from a user (receiveFrom) and displays it to the recipient (sendTo). So in its definition, it calls the receiveMessage function for the user who is supposed to receive the message.

## When to use the mediator pattern?
It can be used:

- If your system has multiple parts that need to communicate

- To avoid tight coupling of objects in a system with a lot of objects

- To improve code readability

- To make code easier to maintain

If communication between objects becomes complex and hinders the reusability of code
