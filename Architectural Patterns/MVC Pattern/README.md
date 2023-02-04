# MVC Pattern
## What is the MVC pattern?
The MVC pattern stands for model view controller pattern. It is an architectural pattern used to organize the code of your application. It consists of three components:

- Model:

This is the model component that manages the data that the application may require.

- View:

The view is used for the visual representation of the current model. It renders data on the user’s side.

- Controller:

The controller connects the model and the view components.

The model is independent of the view, meaning, it is not concerned with the user interface and how the information is displayed on the user side. The view, on the other hand, is an observer of the model. Whenever the model gets modified (data is updated), it notifies its observer (the view) which then reacts accordingly.

As mentioned, view is the visual representation of the model. Whenever it is notified of a change in the model, the view updates correspondingly. As the view layer is what the users get to see, this is also the layer they get to interact with, such as editing or updating attribute values.

The controller is the connection between the model and the view. The controller takes input from the user such as a click or keypress which updates the view side and then updates the model. It can sometimes update the view directly as well.

[MVC pattern concept](./concept.jpg)



## Example
```javascript
class EmployeeModel 
{ 
    constructor(name, designation, id){
        this.name = name;
        this.designation = designation;
        this.id = id;
    }
    
    getDesignation()  
    { 
        return this.designation
    } 
    getID(){
        return this.id
    }
     
    getName()  
    { 
        return this.name
    } 
} 

class EmployeeView  
{
    constructor(){
        this.controller = null;
    }
    registerWith(controller) {
        this.controller = controller;
        this.controller.addView(this); 
    }
    
    printEmployeeInfo( name, designation, id) 
    { 
        console.log(`Employee info:\nName is: ${name}\nID is: ${id}\nDesignation is: ${designation}`); 
    } 
    hire(name, designation) {
        this.controller.hire(name, designation);
    }
    editName(id,name){
        this.controller.setEmployeeName(id,name);       
    }
} 

class EmployeeController  
{ 
    constructor(){
        this.model = null;
        this.view = null;
        this.empList = [];
    }
    
    addView(view) {
        this.view = view;
    }
    addModel(model) {
        this.model = model;
    }
    setEmployeeName(id,name){
        if(this.empList[id]){
            this.empList[id].name = name;
            this.updateView();
        }else{
            console.log("Incorrect id");
        } 
    }
  
    updateView() 
    {
        console.log("List of employees:")
        
        for( let i in this.empList)      
            this.view.printEmployeeInfo(this.empList[i].getName(), this.empList[i].getDesignation(), this.empList[i].getID()); 
        console.log("\n");
    }     
    hire(name, designation) {
        this.empList.push(new EmployeeModel(name, designation, this.empList.length));
        this.updateView();
    }
} 
var view = new EmployeeView();
var controller = new EmployeeController();
view.registerWith(controller);
console.log("Hiring a new employee Rachel");
view.hire("Rachel", "Team Lead");
console.log("Hiring a new employee Jack");
view.hire("Jack", "Software Engineer");
console.log("Updating the name of employee with id 0");
view.editName(0,"Monica");
console.log("Updating the name of an employee with id 7");
view.editName(7,"Joey");
```

### Explanation
The code snippet above is a basic MVC pattern example. This program simply displays the information of the employees hired. As discussed, it is divided into three components:

The employee object is represented by the model EmployeeModel

The employee’s information is displayed by the view component EmployeeView

The model and view components are connected through the controller EmployeeController

Let’s take a look at each component one by one.

- EmployeeModel

```javascript
class EmployeeModel 
{ 
  constructor(name, designation, id){
      this.name = name;
      this.designation = designation;
      this.id = id;
  }

  getDesignation()  
  { 
    return this.designation
  }

  getID()
  {
    return this.id
  }

  getName()  
  { 
    return this.name
  } 
} 
```
The EmployeeModel presents an employee object which has the following properties: the name, designation, and id of the current employee. It also has get functions for these properties to retrieve the values of these properties.

- EmployeeView
```javascript
class EmployeeView  
{
  constructor(){
     this.controller = null;
   }
 //code...
} 
```
The view is responsible for displaying the data, that is, the employee information on the user interface. It must show the updated information if a change occurs in the data in the model or if a user makes an edit in the view when interacting with the interface.

As discussed, the controller acts as the mediator between the model and the view. Hence, for the controller to be able to communicate with the view, we initialize the current view for the controller by first setting the controller property and then, adding the view to it:
```javascript
registerWith(controller) {
  this.controller = controller;
  this.controller.addView(this);
}
```
Next, let’s look at the scenario where a user makes an edit in the view. In this case, let’s assume the HR fills the information of an employee (name & designation) and invokes the hire function in view (you can think of it as a button at the end of the form). Now that there has been an edit in the view, the controller will register that user action and make changes on the model side accordingly.
```javascript
hire(name, designation) {
      this.controller.hire(name, designation);
}
```
We’ll discuss the changes the controller makes in detail, later on. For now, all you need to know is that as a result of the user action on the view side, the controller invoked its own hire function to make updates at the model side.

Another scenario is where the data in the model changes and the view updates itself in a corresponding manner. For example, let’s say, HR changes the name of an existing employee. This change in the model will need to be reflected on the view side. Here again, the controller will act as the mediator between the two:
```javascript
 editName(id,name){
   this.controller.setEmployeeName(id,name);       
}
```
We’ll look into how the updated information of an employee is communicated and reflected on the view side later on.

Finally, since the view needs to display the information, it also has a print function that displays the name, id, and designation of the employee.
```javascript
printEmployeeInfo( name, designation, id) 
{ 
  console.log(`Employee info:\nName is: ＄{name}\nID is: ＄{id}\nDesignation is: ＄{designation}`); 
} 
```


- EmployeeController
```javascript
class EmployeeController  
{ 
  constructor(){
      this.model = null;
      this.view = null;
      this.empList = [];
  }
  
  addView(view) {
      this.view = view;
  }
  addModel(model) {
      this.model = model;
  }
  //code...
}  
```
The controller has the methods addView and addModel to initialize the model and the view components. It also initializes the property empList, which stores the list of all employee model objects.

The controller has a hire function of its own that it calls when the HR person calls the hire function on the view side.
```javascript
hire(name, designation) {
      this.empList.push(new EmployeeModel(name, designation, this.empList.length));
      this.updateView();
}
```
The function creates a new instance of the EmployeeModel using the name and designation of the person the HR wants to hire. It then adds the new employee into the empList. Now that a new employee has been added to the list, the controller updates the view by making a call to the updateView function.

Let’s look at how the updateView function is defined:
```javascript
updateView() 
{
  console.log("List of employees:")
      
 for( let i in this.empList)      
       this.view.printEmployeeInfo(this.empList[i].getName(), this.empList[i].getDesignation(), this.empList[i].getID()); 
  console.log("\n");
}    
```
The function iterates through the list of employees and calls the printEmployeeInfo function in view for each employee.

Next, the controller also has a setEmployeeName function:
```javascript
setEmployeeName(id,name){
   if(this.empList[id]){
     this.empList[id].name = name;
     this.updateView();
    }else{
       console.log("Incorrect id");
    } 
} 
```
Whenever data (name) in the employee model is to be updated by the HR, the setEmployeeName function in the controller is invoked. It finds the id of the employee whose information is to be changed. If the employee exists, it accesses that employee model and changes its name property. When the data in the model changes, the view is also updated by invoking the updateView function. If the wrong id is accessed, the message Incorrect id is displayed.

## When to use the MVC pattern?
You can use this pattern if you want:

- improved application organization in your application

- faster development so that developers can work on different components of the application simultaneously

- to develop an application that loads fast as MVC supports asynchronous technique

- multiple views for the model

- to increase the scalability of the application as modification in separate components is easier
