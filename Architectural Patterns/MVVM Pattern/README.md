# MVVM Pattern
## What is the MVVM pattern?
The MVVM pattern stands for model view viewModel pattern. It is based on the MVC and MVP patterns discussed in the previous lessons. It is used to further separate the working of the user interface from the business logic in the application.

Let’s look at the components in the MVVM pattern.

- Model: As seen in the MVC and MVP patterns, the model stores all the data and information required by the application. As you know, the model does not interfere with how this data will be manipulated or displayed.

- View: The view displays the information on the interface. It can also accept user input, hence, it contains behavior. In the MVVM pattern, the views aren’t passive. Passive views are manipulated by the controller or presenter and are responsible for displaying the information without having any knowledge of the model. However, in MVVM, views are active. They contain data-bindings, behavior, and events that require the knowledge of model and ViewModel. The view handles its events and it doesn’t depend on the ViewModel entirely. However, it does not maintain its state, for that, it syncs up with the ViewModel.

- ViewModel: Similar to the controller in the MVC, the ViewModel acts as the connection between the model and the view. It converts information from the model format to the view format for display. For example, the model might have a date stored in Unix format, whereas the view might display it in another format. Here, the ViewModel will help in converting the information. It also updates the model when a user action on the view occurs and is used to pass commands from view to the model. It is also used to maintain the view’s state and trigger events on it.

### View and ViewModel
The view and ViewModel communicate via events, data binding, and method calls. While view maps its events to the ViewModel through commands, the ViewModel exposes model properties that are updated by the view through two-way data binding.

### Model and ViewModel
ViewModel exposes the model and its properties for data binding. It also contains interfaces to fetch and format the properties it displays to the view.

[MVVM pattern concept](./concept.jpg)

From this diagram, you can tell that the MVVM pattern is based on the observer pattern. The model is the subject here that notifies the ViewModel (observer) of any changes. The ViewModel then updates the view accordingly. Any changes in the view are also reflected in the model via the ViewModel.

## Example

```javascript
//MODEL
class Model{
	constructor(){
		this.model  = {name : "Stuart"};
		this.observers = [];
	}
	subscribe(observer){
		this.observers.push(observer);
	}
	notifyObservers(attrName, newVal){
		for(var i = 0; i < this.observers.length; i++){
			this.observers[i](attrName, newVal);
		}
	}
	getCurrentName(nameKey){
		console.log(this.model[nameKey]);
		return this.model[nameKey];
	}
	
	setNameValue(nameKey, value){
		this.model[nameKey] = value;
		this.notifyObservers(nameKey, value);
	}
}


//VIEWMODEL  
class ViewModel{
	constructor(model){
		this.bind = function(viewElement, modelElement){
		    viewElement.value = model.getCurrentName(modelElement);
			model.subscribe(function(attrName, newValue){
				document.getElementsByName(attrName).forEach(function(elem){
					elem.value = newValue.toUpperCase();
				});
			});
			viewElement.addEventListener('input', function(){
				model.setNameValue(viewElement.name, viewElement.value);
		});
	  }
	}
}

//VIEW
var nameInput = document.getElementById('name');
var nameCopy = document.getElementById('nameCopy');
var model = new Model()
var viewModel = new ViewModel(model);
viewModel.bind(nameInput, 'name');
viewModel.bind(nameCopy, 'name');
```


```html
<html>
	<head>
	</head>
	<body>
		Name: <input type="text" name = "name" id="name"> <input type="text" name = "name" id="nameCopy"><br>
	</body>
<html>
```


### Explanation
This example implements the MVVM pattern to display some names in the input fields. Entering the name in one field shows it in the other input field as well in capital letters. This program is divided into three components: the model, ViewModel, and view. Let’s take a look at them one by one.

**Model**
```javascript
class Model{
    constructor(){
        this.model  = {name : "Stuart"};
        this.observers = [];
    }
   //code...
}
```
The Model constructor consists of the following properties:

model: the object containing the name in the input field. This name is set to Stuart in the beginning.

observers: this is the list containing all the observers of the model.

Since the Model is a subject, it also contains the subscribe function to register an observer.
```javascript
subscribe(observer){
    this.observers.push(observer);
}
```
It contains the notifyObservers function, which informs all the observers of the changes that occur in the model. If you look at the code below, you’ll see that it calls each observer function, passing it the updated value in the Model.
```javascript
notifyObservers(attrName, newVal){
    for(var i = 0; i < this.observers.length; i++){
    this.observers[i](attrName, newVal);
    }
}
```
It has the getCurrentName function to retrieve the current name value from the model.
```javascript
getCurrentName(nameKey){
    return this.model[nameKey];
}
```
It also has the setNameValue, which can be used to set the value of the name attribute in the model.
```javascript
setNameValue(nameKey, value){
    this.model[nameKey] = value;
    this.notifyObservers(nameKey, value);
}
```
As you can see, when the new value is set, the notifyObservers function is called to inform the observers of this change. But what is the observer here? Our ViewModel is the observer that looks out for any changes in the model so it can update the view accordingly. So let’s discuss it next.

ViewModel
```javascript
class ViewModel{
    constructor(model){
        this.bind = function(viewElement, modelElement){
            viewElement.value = model.getCurrentName(modelElement);
            model.subscribe(function(attrName, newValue){
                document.getElementsByName(attrName).forEach(function(elem){
                    elem.value = newValue.toUpperCase();
                });
            });
            viewElement.addEventListener('input', function(){
                model.setNameValue(viewElement.name, viewElement.value);
        });
    }
   }
}
```
The constructor initializes the bind function that binds the model element to the view element. It sets the value of the viewElement equal to the value retrieved from the model (using getCurrentName function). This is the reason you see Stuart in the output by default.

viewElement.value = model.getCurrentName(modelElement);
Next, it calls the subscribe function to which it passes the callback that is invoked when a change in the model occurs. This callback function takes the updated name in the model and sets it as the new value of viewElement after converting it to uppercase.
```javascript
model.subscribe(function(attrName, newValue){
                document.getElementsByName(attrName).forEach(function(elem){
                    elem.value = newValue.toUpperCase();
           });
});
```
The above scenario deals with the case where the changes made in the model are to be reflected on the user interface. The code below caters to when the model needs to reflect the changes made on the UI side.
```javascript
viewElement.addEventListener('input', function(){
        model.setNameValue(viewElement.name, viewElement.value);
});
```
We attach an event listener to the viewElement, so that whenever a user enters a name in any one of the input fields, model.setNameValue function gets called. It takes the view element’s name value as the argument and updates the model element’s name value accordingly. Since both the input entries are bound to the same model, when you enter the name in one of them, the model notifies all the observers of the change and the second input entry modifies itself as well.

View
```html
Name: <input type="text" name = "name" id="name"> <input type="text" name = "name" id="nameCopy"><br>
```
The view is simple HTML code that creates two input fields for the name. This allows the user to interact and enter the name in either of the fields.
```javascript
var nameInput = document.getElementById('name');
var nameCopy = document.getElementById('nameCopy');
var viewModel = new ViewModel();
var model = new Model()
var viewModel = new ViewModel(model);
viewModel.bind(nameInput, 'name');
viewModel.bind(nameCopy, 'name');
```
On the JavaScript side, we get both input elements and store them in nameInput and nameCopy variables. Next, we initialize the viewModel and model and call the view model’s bind function to bind both view elements to the model element name.

## When to use the MVVM pattern?
You can use this pattern if you want:

to display the data stored in the model in a different format on the view side

to slim down the model’s number to view transformations that the controller is handling in MVC

to make your application more maintainable, reusable, and extendable
