# Primitive and Reference Values
* primitive values: actual value stored in the variable
  * variables are accessed by value
* reference values: are objects stored in memory
  * objects are accessed by reference
  
## Dynamic Properties
Only reference values can have properties defined dynamically for later use. <br />
<br />
Here is the example:

```javascript
    // for reference values
    var persion = new Object()
    person.name = "Joe";
    alert(person.name);   // returns "Joe"
    
    // for primitive values
    var name = "Joe";
    name.age = 21;
    alert(name.age);    // undefined
```

## Copying Values
* When a primitive value is assigned from one variable to another, the vlaue stored on the variable object is created and copied into the location for the new variable.

```javascript
    var num1 = 5;
    var num2 = num1;
```

Consider the tables below: <br />
*variable object before copy*

|||
|---|---|
|||
|num1|5 (Number type)|

*variable object after copy*

|||
|---|---|
|num2|5 (Number type)|
|num1|5 (Number type)|

* When a reference value is assigned from one variable to another, the value stored on the variable object is also copied into the location for the new variable. Actually, both the old and new variable are pointing to the same object stored on the heap. So changes to one are reflected on the other.

```javascript
    var obj1 = new Object();
    var obj2 = obj1;
    obj1.name = "Joe";
    alert(obj2.name);   // "Joe"
```

Consider the following tables: <br />
*Variable object before copy*

|||
|---|---|
|||
|obj1|(Object type)|

|||
|---|---|
|obj2|(Object type)|
|obj1|(Object type)|

**Both obj1 and obj2 are pointing to a same object.**

## Argument Passing

## Determining Type
