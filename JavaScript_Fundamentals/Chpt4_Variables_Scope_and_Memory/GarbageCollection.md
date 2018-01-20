# Garbage Collection
The execution environment is responsible for managing the memory required during code execution.

## Mark-and-Sweep
When a variable comes into context, such as when a variable is declared inside a function, it is flagged as being in context.
<br />
Variables are flagged as either **in context** or **out of context**.
## Reference Counting
Problem:

```javascript
    function problem() {
        var objA = new Object();
        var objB = new Object();
        
        objA.someOtherObject = objB;
        objB.anotherObject = objA;
    }
```

objects in the Browser Object Model(BOM) and Document Object Model(DOM) are implemented as COM(Component Object Model) objects in C++, and COM objects use reference counting for garbage collection.<br />
<br />
A circular reference with a COM object:

```javascript
    var element = document.getElementById("some_element");
    var myObject = new Object();
    myObject.element = element;
    element.someObject = myObject;
```

To avoid circular reference problems such as this, you should break the connection between native JavaScript objects and DOM elements when you're finished using them:

```javascript
    myObject.element = null;
    element.someObject = null;
```

## Performance

## Managing Memory
When data is no longer necessary, it's best to set the value to `null`, freeing up the reference - this is called *dereferencing* the value.<br />
This advice applies mostly to global values and properties of global objects.

```javascript
    function createPerson(name) {
        var localPerson = new Object();
        localPerson.name = name;
        return localPerson;
    }
    
    var globalPerson = createPerson("Joe");
    
    // do something with globalPerson
    
    globalPerson = null;
    
    /*
     * Dereferencing a value doesn't automatically
     * reclaim the memory associated with it.
     * The point of dereferencing is to make sure
     * the value is out of context and will be 
     * reclaimed the next time garbage collection
     * occurs.
     */
```
