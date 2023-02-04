# MVP Pattern
## What is the MVP pattern?
The MVP pattern stands for model view presenter. It is derived from the MVC pattern, which focuses on the user interface. MVP however, is focused on improving the presentation logic.

It consists of three components:

- Model: provides the data that the application requires, which we want to display in the view

- View: to display the data from the model, it passes the user actions/commands to the presenter to act upon that data

- Presenter: acts as the middle man between the model and the view. Retrieves data from the model, manipulates it, and returns it to view for display. It also reacts to the user’s interaction with the view.

### What’s the difference between MVP and MVC?

```

                                        MVC                                                                                                            	MVP
The controller acts as the mediator between the model and the view	                                                         The presenter acts as the mediator between the model and the view
Controller can share multiple views	                                                                                         The presenter has a one to one mapping with the view. If the view is complex, it can use multiple presenters
The view can also communicate directly with the model by observing it for changes and updating itself accordingly	The        model and the view are more separate in this pattern as the presenter reacts to user actions by retrieving and manipulating model data and returning it to the view for display
```
[MVP pattern concept](./concept.jpg)

## Example
```javascript
class Model{
    constructor(text){
        this.text = text;
    }
    setText(text){
        this.text = text;
    }
    getText(){
        return this.text;
    }
}

class View{
    constructor(){
        this.presenter = null;
    }

    registerWith(presenter){
        this.presenter = presenter;
    }

    displayError(){
        console.log("Text is not in upper case");
    }

    displayMessage(text){
        console.log("The text is: " + text);
    }

    changeText(text){
        this.presenter.changeText(text);
    }
}

class Presenter{
    constructor(view){
        this.view = view;
        this.model = null;
    }

    setModel(model){
        this.model= model;
    }

    getView(){
        return this.view;
    }

    changeText(text){
        if(text !== text.toUpperCase()){
            this.view.displayError();
        }else{
            this.model.setText(text); 
            this.view.displayMessage(this.model.getText());
        }
    }
}

var model   = new Model("Hello world!")
var view = new View()
var presenter = new Presenter(view)
presenter.setModel(model)
view.registerWith(presenter)
presenter.getView().changeText("unagi")
presenter.getView().changeText("UNAGI")
```

### Explanation
The example above implements the MVP pattern to display some text. It has three components, the Model, View, and Presenter. Let’s discuss them one by one.

**Model**
```javascript
class Model{
    constructor(text){
        this.text = text;
    }
    setText(text){
        this.text = text;
    }
    getText(){
        return this.text;
    }
}
```
The Model contains the data which includes the text to be displayed. It also has two functions: setText and getText to set and retrieve the text property.

**View**
```javascript
class View{
    constructor(){
        this.presenter = null;
    }

    registerWith(presenter){
        this.presenter = presenter;
    }

    displayError(){
        console.log("Text is not in upper case");
    }

    displayMessage(text){
        console.log("The text is: " + text);
    }

    changeText(text){
        this.presenter.changeText(text);
    }
}
```

**Presenter**
```javascript
class Presenter{
    constructor(view){
        this.view = view;
        this.model = null;
    }

    setModel(model){
        this.model= model;
    }

    getView(){
        return this.view;
    }

    changeText(text){
        if(text !== text.toUpperCase()){
            this.view.displayError();
        }else{
            this.model.setText(text); 
            this.view.displayMessage(this.model.getText());
        }
    }
}
```
As discussed, the Presenter is the channel of communication between the model and the view. Hence, it initializes the view in its constructor and uses the setModel function to initialize the model.

The presenter exposes the getView function to return the view set in the constructor. Now, if a user tries to change the text in the view mode, the view relays this action to the presenter, which then executes its changeText function.

changeText checks whether the text that the user wants to set is in the upper case or not. If it is, it updates the model and sets the new text. Since the model gets updated, the function returns the updated data to the view for display:
```javascript
this.model.setText(text);
this.view.displayMessage(this.model.getText()) 
```
However, if the text is not in the upper case, it calls the view to display the error.
```javascript
if(text !== text.toUpperCase()){
    this.view.displayError();
```    
As discussed, the View is responsible for passing any user actions to the presenter. An example is that of the changeText function, which allows the user to change the text. As you can see, it notifies the presenter, which then calls the changeText function of its own. We will get into that when we discuss the Presenter. Similarly, the View also displays the updated data returned to it by the presenter. The displayError and displayMessage functions are defined for that purpose.

[Example](./example.jpg)

## When to use the MVP pattern?
You can use this pattern:

- If your application requires a lot of reuse of the presentation logic

- If your application requires a lot of user interaction

- If your application has complex views

- For easier testing as the presenter can provide a mock interface that can be unit tested
