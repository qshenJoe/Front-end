# Functions
Functions in ECMAScript are declared using the `function` keyword, followed by a set of arguments and then the body of the function. <br />
Syntax:

```javascript
    function functionName(arg0, arg1, ..., argN) {
        statements
    }
    
    // Here's the example
    function sayHi(name, message) {
        alert("Hello " + name + ", " + message);
    }
    
    // The above function can be called like this:
    sayHi("Joe", "how are you?");
    
    // return a value
    function sum(num1, num2) {
        return num1 + num2;
    }
    
    var result = sum(1, 5);
```

**A function stops executing and exits immediately when it encounters the `return` statement.**

```javascript
    function sum(num1, num2) {
        return num1 + num2;
        alert(num1 + num2);   // never executed
    }
```
If no return value is specified, an `undefined` value will be returned.

## Understanding Arguments
Arguments in ECMAScript are represented as an array internally and can be accessed by referencing `arguments[index]`

```javascript
    function sayHi() {
        alert("Hello " + arguments[0] + ", " + arguments[1]);
    }
```

**Named arguments are a convenience, not a necessity.**

```javascript
    function howManyArgs() {
        alert(arguments.length);
    }
    
    howManyArgs("string", 21);  // 2
    howManyArgs();    // 0
    howManyArgs(21);    // 1
```

`arguments` object can also be used in conjunction with named arguments:

```javascript
    function doAdd(num1, num2) {
        if (arguments.length == 1) {
            alert(num1 + 10);
        } else if (arguments.length == 2) {
            alert(arguments[0] + num2);
        }
    }
```
**All arguments in ECMAScript are passed by value. It is not possible to pass arguments by reference.**

## No Overloading
If two functions have the same name, the last function will be the owner of that name:

```javascript
    function addSomeNumber(num) {
        return num + 100;
    }
    
    function addSomeNumber(num) {
        return num + 200;
    }
    
    var result = addSomeNumber(100);    // 300
```
