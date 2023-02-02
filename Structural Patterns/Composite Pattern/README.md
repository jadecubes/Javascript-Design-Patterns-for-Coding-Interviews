# Composite Pattern
## What is the composite pattern?
The composite pattern is used to structure objects in a tree-like hierarchy. Here, each node of the tree can be composed of either child node(s) or be a leaf (no children objects). This pattern allows the client to work with these components uniformly, that is, a single object can be treated exactly how a group of objects is treated.

This pattern allows the formation of deeply-nested structures. If a leaf object receives the request sent by the client, it will handle it. However, if the recipient is composed of children, the request is forwarded to the child components.

[Composite pattern concept]

From the diagram, you can see that a composite pattern consists of the following:

- Component: an abstract class that contains methods such as add, remove, get that are used in managing the children. The component can be a leaf object or composite.

- Composite: it is the subclass that implements a component. It is composed of other components (children).

- Leaf: it is the subclass that implements a component. It does not have children.

Let’s take a look at an example to see its implementation.

## Example
```javascript
//Component
class Employee{
    constructor(name,position,progress){
        this.name = name
        this.position = position
        this.progress = progress
    }
    getProgress(){
    }
}

//Leaf subclass
class Developers extends Employee{
    constructor(name,position,progress){
        super(name,position,progress)
    }
    getProgress(){
        return this.progress
    }
}

//Leaf subclass
class FreeLanceDev extends Employee{
    constructor(name,position,progress){
        super(name,position,progress)
    }
    getProgress(){
        return this.progress()
    }
}

//Composite subclass
class DevTeamLead extends Employee{
    constructor(name,position){
        super(name,position)
        this.teamMembers = []
    }
    addMember(employee){
        this.teamMembers.push(employee)
    }

    removeMember(employee){
       for(var i=0; i<this.teamMembers.length; i++){
           if(this.teamMembers[i] == employee){
               this.teamMembers.splice(i,1)
           }
       }
       return this.teamMembers
    }

    getProgress(){
         for(var i=0; i<this.teamMembers.length; i++){
            console.log(this.teamMembers[i].getProgress())
        }
    }
 
    showTeam(){
        for(var i=0; i<this.teamMembers.length; i++){
            console.log(this.teamMembers[i].name)
        }
    }
}

const seniorDev = new Developers("Rachel","Senior Developer","60%")
const juniorDev = new Developers("Joey","Junior Developer", "50%")
const teamLead = new DevTeamLead("Regina", "Dev Team Lead","90%")
teamLead.addMember(seniorDev)
teamLead.addMember(juniorDev)
console.log("Team members list:")
teamLead.showTeam()
console.log("Get Team members progress:")
teamLead.getProgress()
console.log("Removing Rachel from team:")
teamLead.removeMember(seniorDev)
console.log("Updated team members list:")
teamLead.showTeam()
const freelanceDev = new Developers("Ross", "Free Lancer", "80%")
console.log("Get freelance developer's progress:")
console.log(freelanceDev.getProgress()) 
```
### Explanation
The illustration below provides a visualization of the hierarchy of the objects in the code above:

From the illustration above, you can see the hierarchy of the objects. The pattern consists of the following:

Component: Employee class that includes the abstract method, getProgress, and properties of an employee such as name, position, and work progress.

Leaf: Developers and FreeLanceDev are both Employee's subclasses. They are the leaf components as they have no children. Both have a getProgress function that returns the progress of the developer/freelancer.

Composite DevTeamLead is the composite subclass. It has other developers as its children.

Let’s start to discuss each component.


**Employee**
```javascript
//Component 
class Employee{
    constructor(name,position,progress){
        this.name = name
        this.position = position
        this.progress = progress
    }
    getProgress(){
    }
}
```
An Employee object will contain the properties: name, position, progress, and the method getProgress.


**Developers**
```javascript
//Leaf subclass
class Developers extends Employee{
    constructor(name,position,progress){
        super(name,position,progress)
    }
    getProgress(){
        return this.progress
    }
}
```
Developers is the leaf subclass of Employee. The super command initializes the constructor of Employee setting the name, age, and position of the Developer instance.

It also redefines the getProgress function by returning the work progress of the developer.


**FreeLanceDev**
```javascript
//Leaf subclass
class FreeLanceDev extends Employee{
    constructor(name,position,progress){
        super(name,position,progress)
    }
    getProgress(){
        return this.progress()
    }
}
```
FreeLanceDev is the leaf subclass of Employee. The super command initializes the constructor of Employee, setting the name, age, and position of the FreeLanceDev instance.

It also redefines the getProgress function by returning the work progress of the freelance developer.



**DevTeamLead**
```javascript
class DevTeamLead extends Employee{
    constructor(name,position){
        super(name,position)
        this.teamMembers = []
    }
  //code...
}
```
The DevTeamLead is the composite subclass of Employee. The super command initializes the constructor of Employee, setting the name, and position of the DevTeamLead instance. It also initializes the teamMembers variable, which stores the list of members working in the dev lead’s team.

A dev team lead can add members to his team as well as remove them. So, it contains the additional functions for adding/removing members:
```javascript
class DevTeamLead extends Employee{
  //code...
  addMember(employee){
     this.teamMembers.push(employee)
   }

  removeMember(employee){
   for(var i=0; i<this.teamMembers.length; i++){
     if(this.teamMembers[i] == employee){
        this.teamMembers.splice(i,1)
      }
    }
     return this.teamMembers
  }
}
```
Its getProgress function is defined as follows:
```javascript
getProgress(){
    
for(var i=0; i<this.teamMembers.length; i++){
   
 console.log(this.teamMembers[i].getProgress())
   }
}
```
It returns the progress of every team member in the dev lead’s team.



Finally, it contains the showTeam() function, which lists the name of all the members in the dev lead’s team.
```javascript
showTeam(){
        for(var i=0; i<this.teamMembers.length; i++){
           console.log(this.teamMembers[i].name)
   }
   ```
So now, we can make a developer. The team lead can add or remove them from the team. The lead will be able to access their progress as well.
```javascript
const seniorDev = new Developers("Rachel","Senior Developer","60%")
const teamLead = new DevTeamLead("Regina", "Dev Team Lead","90%")
teamLead.addMember(seniorDev)
teamLead.showTeam()
teamLead.getProgress()
teamLead.removeMember(seniorDev)
```
Note that the freelanceDev is not a part of the team. However, his progress can be accessed the same way we access the progress for a composite subclass.
```javascript
const freelanceDev = new Developers("Ross", "Free Lancer", "80%")
console.log(freelanceDev.getProgress()) 
```
Hence, with this pattern, single objects and composite objects can be treated uniformly. Here, we call getProgress on a single object the same way we call it on a composite object.

## When to use the composite pattern?
The composite pattern is powerful as it allows us to treat an object as a composite. Since both single and composite objects share the same interface, it allows reusing objects without worrying about their compatibility.

You can use this pattern if you want to develop a scalable application that uses plenty of objects. It is particularly helpful in situations where you are dealing with a tree-like hierarchy of objects. An example of this pattern is being used by your operating system to create directories and subdirectories. Libraries like React and Vue also use this pattern to build reusable interfaces.

