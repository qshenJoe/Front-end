# The Object Type
There are two ways to explicitly create an instance of `Object`.
* Use the `new` operator with the `Object` constructor:

```javascript
    var person = new Object();
    person.name = "Joe";
    person.age = 21;
```

* Use *object literal* notation

```javascript
    var person = {
        name : "Joe",
        age : 21    // do not include a comma after the last property
    };
```

The left curly brace({) signifies the beginning of an object literal, because it occurs in an *expression context*.

```javascript
    var person = {
        "name" : "Joe",
        "age" : 21,
        5: true   // numeric property names are automatically converted to strings
    };
    
    // Property names can also be specified as strings or numbers when using object literal notation
```

It's also can be done like this:

```javascript
    var person = {};    // same as new Object()
    person.name = "Joe";
    person.age = 21;
```

**When defining an object via object literal notation, the `Object` constructor is never actually called.**
<br />

```javascript
    function displayInfo(args) {
        var output = "";
        
        if (typeof args.name == "string") {
            output += "Name: " + args.name + "\n";
        }
        
        if (typeof args.age == "number") {
            output += "Age: " + args.age + "\n";
        }
        
        alert(output);
    }
    
    displayInfo({
        name: "Joe",
        age: 21 
    });
    
    displayInfo({
        name: "Qiaohong"
    });
```

Object properties can be accessed using both:
* dot notation
* bracket notation

```javascript
    alert(person["name"]);    // "Joe"
    // The advantage of bracket notation is that it allows you to use variables for property access
    var propertyName = "name";
    alert(person[propertyName]);    // "Joe"
    
    person["first name"] = "Joe";
    
    alert(person.name);   // "Joe"
```

Generally speaking, dot notation is preferred unless variables are necessary to access properties by name.
