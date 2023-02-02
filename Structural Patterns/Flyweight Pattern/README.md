# Flyweight Pattern
## What is the flyweight pattern?
It is a structural pattern that focuses on how related objects share data. It helps prevent repetitive code and increases efficiency when it comes to data sharing as well as conserving memory.

This pattern takes the common data structures/objects that are used by a lot of objects and stores them in an external object (flyweight) for sharing. You could say that it is used for caching purposes. So, the same data does not need to have separate copies for each object, instead, it is shared amongst all.

A flyweight is an independent object that can be used in multiple contexts simultaneously. It cannot be distinguished from the instances of objects that are not sharable. A flyweight object can consist of two states:

- intrinsic: this state is stored in the flyweight. It contains the information required by the internal methods of objects. It is independent of the context of the flyweight and is sharable with other objects.

- extrinsic: this state depends on the context of the flyweight and it cannot be shared. Normally, the client objects pass the extrinsic state to the flyweight object when needed.

[Flyweight pattern concept]

## Example
Let’s start by considering a program that doesn’t use the flyweight pattern.

```javascript
class CodeFile {
    constructor(codefileName){
        this.codefileName = codefileName
    }
}

 
class Formatter {
    format(codefile){}
}

 
class PythonFormatter extends Formatter {
 
    constructor(){
        super()
        console.log("Python Formatter instance created")
    }
     
   
    format(codefileName) {
        console.log(`"Formatting the Python ${codefileName} file you uploaded.`)
    }
 
}
const codefile = new CodeFile("helloworld.py")
const pythonFormatter = new PythonFormatter()
pythonFormatter.format(codefile.codefileName)
```

The code above implements an online code formatter platform without using the flyweight pattern. The Formatter is available for different languages such as Python, C++, JavaScript, Java, etc. In the example above, we have its child class, PythonFormatter.

So, a user will come online, upload their .py file, and choose the PythonFormatter to format it. You have two objects here: a CodeFile and a PythonFormatter object.

Now, imagine 1,000 users using this platform to format their .py files. They will have different files, so there will be 1000 different CodeFile objects. However, there will also be 1,000 instances of the PythonFormatter, which is comparatively heavier since it involves the environment setup. It will be a more time-consuming task as well. Wouldn’t it be better to have control over the creation of the PythonFormatter instances? This is where the flyweight pattern comes in.

Let’s look at the updated code:

```javascript
class CodeFile {
    constructor(codefileName){
        this.codefileName = codefileName
    }
}

 
class Formatter {
    format(codefile){}
}

 
class PythonFormatter extends Formatter {
 
    constructor(){
        super()
        console.log("Python Formatter instance created")
    }
     
   
    format(codefileName) {
        console.log(`"Formatting the Python ${codefileName} file you uploaded.`)
    }
 
}

class JavaFormatter extends Formatter {
 
    constructor(){
        super()
        console.log("Java Formatter instance created")
    }
     
   
    format(codefileName) {
        console.log(`"Formatting the Java ${codefileName} file you uploaded.`)
    }
 
}


class FormatterFactory {
  constructor() {
    this.myFormatterMap = new Map()
  }

  createFormatter(formatterType) {
    let formatter = this.myFormatterMap.get(formatterType)
    if (formatter == null) {
      if(formatterType == "Python"){
        formatter = new PythonFormatter()
      }
      else if(formatterType == "Java"){
        formatter = new JavaFormatter()
      }
      this.myFormatterMap.set(formatterType, formatter);
    }
    return formatter
  }
}

const codefile1 = new CodeFile("helloworld.py")
let formatter = new FormatterFactory()
const pythonFormatter = formatter.createFormatter("Python")
pythonFormatter.format(codefile1.codefileName)
//uploading new codefile Python file
const codefile2 = new CodeFile("test.py")
const anotherPythonFormatter = formatter.createFormatter("Python")
anotherPythonFormatter.format(codefile2.codefileName)
console.log("Both Python Formatter instances are the same? " + (anotherPythonFormatter === pythonFormatter))
//uploading a Java file
const codefile3 = new CodeFile("myfile.java")
const javaFormatter = formatter.createFormatter("Java")
javaFormatter.format(codefile3.codefileName)
```


### Explanation
In the code above, we used the flyweight pattern by introducing the FormatterFactory class. Whenever an instance of a language formatter is requested, it checks if the instance already exists or not. If it does, it returns the same one. Else, it creates a new instance.

So now, if 1,000 users come online to format a Python file, 1,000 instances of the same formatter need not be created. Instead, a single instance will be shared by everyone.

The formatter instance created in this example, that is, the Python or Java formatter, is our flyweight object. It contains the function, format used to format all different sorts of code file objects. This means all code file objects will require this function, so instead of creating copies for each, it is accessed from the flyweight object instance.

## When to use the flyweight pattern?
This pattern should be used when your application has plenty of objects using similar data or when memory storage cost is high. JavaScript uses this pattern to share a list of immutable strings across the application.

This pattern is mostly used in applications like network apps or word processors. It can also be used in web browsers to prevent loading the same images twice. The flyweight pattern allows caching of images. Therefore, when a web page loads, only the new images are loaded from the Internet, the already existing ones are fetched from the cache.
