# Command Pattern
## What is the command pattern?
The command pattern allows encapsulation of the requests or operations into separate objects. It decouples the objects that send requests from the objects responsible for executing those requests.

Consider an example where the client is accessing the methods of an API directly throughout the application. What will happen if the implementation of that API changes? The change will have to be made everywhere the API is being used. To avoid this, we could make use of abstraction and separate the objects requesting from those implementing the request. Now, if a change occurs, only the object making the call will need to change.

[Command Pattern concept]

The diagram above captures the essence of the command pattern:

- Invoker: asks the command to carry out the request

- Command: has information about the action and binds it to the receiver by invoking the corresponding operation on it

- Receiver: knows how to perform the operations associated with the command

- Client: creates a command and sets the receiver who’ll receive the command

Let’s map the diagram above to a real-life example so you can understand it better:

[Command pattern example]

## Example
```javascript
class Command {
  execute() {};
}

//TurnOnPrinter command
class TurnOnPrinter extends Command {
    
    constructor(printingMachine) {
        super();
        this.printingMachine = printingMachine;
        this.commandName = "turn on" 
    }
    
    execute() {
        this.printingMachine.turnOn();
    }
}

//TurnOffPrinter command
class TurnOffPrinter extends Command {

  constructor(printingMachine) {
    super();
    this.printingMachine = printingMachine;
    this.commandName = "turn off" 
  }
  
  execute() {
    this.printingMachine.turnOff();
  }
  
}

//Print command
class Print extends Command {

  constructor(printingMachine) {
    super();
    this.printingMachine = printingMachine;
    this.commandName = "print" 
  }
  
  execute() {
    this.printingMachine.print();
  }
  
}

//Invoker
class PrinterControlPanel {
    pressButton(command) {
        console.log(`Pressing ${command.commandName} button`);
        command.execute();
    }
}

//Reciever: 
class PrintingMachine {

  turnOn() {
    console.log('Printing machine has been turned on');
  }
  
  turnOff() {
    console.log('Printing machine has been turned off');
  }

  print(){
      console.log('The printer is printing your document')
  }
}


const printingMachine = new PrintingMachine();
const turnOnCommand = new TurnOnPrinter(printingMachine);
const turnOffCommand = new TurnOffPrinter(printingMachine);
const printCommand = new Print(printingMachine)
const controlPanel = new PrinterControlPanel();
controlPanel.pressButton(turnOnCommand);
controlPanel.pressButton(turnOffCommand);
controlPanel.pressButton(printCommand);
```
### Explanation
In the example above, we have a PrintingMachine.
```javascript
class PrintingMachine {

  turnOn() {
    console.log('Printing machine has been turned on');
  }
  
  turnOff() {
    console.log('Printing machine has been turned off');
  }

  print(){
      console.log('The printer is printing your document')
  }
}
```
We can perform the following operations with the printingMachine:

turnOn: turn on the machine

turnOff: turn off the machine

print: print a document using the machine

Whenever the printing machine receives a command for either of these operations, it executes them. From this, we know that there are three types of commands a user can send to the printer:
```javascript
class TurnOnPrinter extends Command {/*code*/}

class TurnOffPrinter extends Command {/*code*/}

class Print extends Command {/*code*/}
All three classes extend the abstract Command class:

class Command {
  execute() {};
}
```
The child classes inherit the execute function and define it accordingly. Let’s look at each command one by one below:
```javascript
class TurnOnPrinter extends Command {
    
    constructor(printingMachine) {
        super();
        this.printingMachine = printingMachine;
        this.commandName = "turn on" 
    }
    
    execute() {
        this.printingMachine.turnOn();
    }
}
```
The constructor takes the printingMachine as a parameter, as the command will be sent to this machine. It also initializes the variable commandName, which is set to turn on.

Next, it defines the execute function, which will perform the task of turning on the printing machine when invoked.

The commands: TurnOffPrinter and Print have similar definitions. For the TurnOffPrinter command, the commandName variable is set to turn off and for the Print command, it is set to print.
```javascript
class TurnOffPrinter extends Command {
   //code...
   this.commandName = "turn off" 
   //code..
}

class Print extends Command {
   //code...
   this.commandName = "print" 
   //code..
}
```
Similarly, they define the execute function, which turns off the machine when the TurnOffPrinter command is executed and prints when the Print command is executed.
```javascript
class TurnOffPrinter extends Command {
   //code...
   execute() {
     this.printingMachine.turnOff();
   }
}

class Print extends Command {
   //code...
   execute() {
     this.printingMachine.print();
   }
}
```
So, how are these commands invoked? The invoker is the control panel of the printer. It has the turn on, turn off, and print buttons that the user will press to send a command.
```javascript
class PrinterControlPanel {
    pressButton(command) {
        console.log(`Pressing ＄{command.commandName} button`);
        command.execute();
    }
}
```
A user will press the button for the command they want to execute. Let’s look at an example:
```javascript
controlPanel.pressButton(turnOnCommand);
```
Here, the user presses the button for turning the printer on. When the button is pressed, the execute function for this command will get executed, and you’ll see the following message: Printing machine has been turned on.


## When to use the command pattern?
You can use it if you want to:

- queue and execute requests at different times

- perform operations such as reset or undo

- keep a history of requests made
