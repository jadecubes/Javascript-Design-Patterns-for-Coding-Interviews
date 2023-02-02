# Proxy Pattern
## What is the proxy pattern?
As the name implies, the proxy pattern is a structural pattern that creates a proxy object. It acts as a placeholder for another object, controlling the access to it.

Usually, an object has an interface with several properties/methods that a client can access. However, an object might not be able to deal with the clients’ requests alone due to heavy load or constraints such as dependency on a remote source that might cause delays (e.g., network requests). In these situations, adding a proxy helps in dividing the load with the target object.

The proxy object looks exactly like the target object. A client might not even know that they are accessing the proxy object instead of the target object. The proxy handles the requests from the clients and forwards them to the target object, preventing undue pressure on the target.

[Proxy pattern concept]

The proxy can also act as a cache and store the requests. When the same request is made again, it can just return it from the cache rather than forwarding it to the target. This allows the target to deal with a lesser number of requests.

[Proxy pattern and caching concept]

## Example

```javascript
class GetCapital{
   getMycapital(country) {
        if (country === "Pakistan") {
            return "Islamabad";
        } else if (country === "India") {
            return "New Delhi";
        } else if (country === "Canada") {
            return "Ottawa";
        } else if (country === "Egypt") {
            return "Cairo";
        } else {
            return "";
        }
    }
}
 
class ProxyGetCapital {
  constructor(){
    this.capital = new GetCapital()
    this.cache = {};
  }

  getMycapital(country){
    if(!this.cache[country]){
      var value = this.capital.getMycapital(country)
      this.cache[country] = value
      return `${value}--Returning From GetCapital`
    }else{
      return `${this.cache[country]}--Returning from Cache`
    }
    
  }
};
 

var capital = new ProxyGetCapital();
console.log(capital.getMycapital("Pakistan"))
console.log(capital.getMycapital("India"))
console.log(capital.getMycapital("Canada"))
console.log(capital.getMycapital("Egypt"))
console.log(capital.getMycapital("Egypt"))
console.log(capital.getMycapital("Egypt"))
console.log(capital.getMycapital("Pakistan"))
console.log(capital.getMycapital("Pakistan"))
console.log(capital.getMycapital("Canada"))
```

### Explanation
This example implements the GetCapital functionality that returns the capital of a country. For the purpose of this example, we have restricted this functionality to four countries: Pakistan, India, Egypt, and Canada.
```javascript
class GetCapital{
   getMycapital(country) {
        if (country === "Pakistan") {
            return "Islamabad";
        } else if (country === "India") {
            return "New Delhi";
        } else if (country === "Canada") {
            return "Ottawa";
        } else if (country === "Egypt") {
            return "Cairo";
        } else {
            return "";
        }
    }
}
```
To reduce the redundant requests received by the GetCapital object, the ProxyGetCapital class is defined. It defines a getMycapital function as well.
```javascript
class ProxyGetCapital {
  constructor(){
    this.capital = new GetCapital()
    this.cache = {};
  }

  getMycapital(country){
    if(!this.cache[country]){
      var value = this.capital.getMycapital(country)
      this.cache[country] = value
      return `＄{value}--Returning From GetCapital`
    }else{
      return `＄{this.cache[country]}--Returning from Cache`
    }
    
  }
};
```
The constructor initializes an instance of the GetCapital class and a cache to store the list of capitals.

Whenever a request is made, the getMycapital function checks the cache to see if the capital is stored, if not, it calls the getMycapital function. Else, it returns the capital from the cache.

Now, if you call getMycapital for the same country twice, for the first time, the result will be returned from the target function. However, the second time, it’ll be returned from the cache.
```javascript
capital.getMycapital("Canada") //Returning From GetCapital
capital.getMycapital("Canada") //Ottawa--Returning from Cache
```
## When to use the proxy pattern?
The proxy pattern tries to reduce the workload on the target object. You can use it when dealing with heavy applications that perform a lot of network requests. Since delays could occur when responding to such requests, using a proxy pattern will allow the target object to not get overburdened with requests.

A real-life example is HTTP requests. These are expensive operations, therefore, the proxy pattern helps in reducing the number of requests forwarded to the target.

When to use the proxy pattern?
