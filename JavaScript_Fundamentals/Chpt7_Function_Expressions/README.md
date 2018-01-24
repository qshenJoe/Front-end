# Function Expressions
There are two ways to define a function:
* by function declaration
* by function expression

```javascript
    /*
     * function declaration
     */
     
    function functionName(arg0, arg1, arg2) {
        // function body
    }
    
    // works only in Firefox, Safari, Chrome, and Opera
    alert(functionName.name);   // "functionName"
```

One of the key characteristic of function declaration is function declaration hoisting, whereby function declarations are read before the code executes.

```javascript
    sayHi();
    function sayHi() {
        alert("Hi");
    }
```

The second way to create a function is by using a function expression.

```javascript
    var functionName = function(arg0, arg1, arg2) {
        // function body
    };
    
    /*
     * The created function is considered to be an anonymous
     * function, because it has no identifier after the 
     * function keyword. (anonymous functions are also called
     * lambda functions.) This means the name property is the
     * empty string.
     */
     
    /*
     * funtion expressions act like other expressions and, 
     * therefore, must be assigned before usage.
     */
     
    sayHi();  // error - function does not exist yet
    var sayHi = function() {
        alert("Hi");
    };
    
    /*
     * Understanding function hoisting is key to understanding the differences 
     * between function declarations and function expressions.
     */
    
    // never do this
    if (condition) {
        function sayHi() {
            alert("Hi!");
        }
    } else {
        function sayHi() {
            alert("Yo!");
        }
    }
    
    /*
     * It is perfectly fine to use function expressions in this way
     */
     
    // this is okay
    var sayHi;
    
    if (condition) {
        sayHi = function() {
            alert("Hi!");
        };
    } else {
        sayHi = function() {
            alert("Yo!");
        };
    }
    
    /*
     * Any time a function is being used as a value,
     * it is a function expression.
     */
     
    function createComparisonFunction(propertyName) {
        
        return function(object1, object2) {
            var value1 = object1[propertyName];
            var value2 = object2[propertyName];
            
            if (value1 < value2) {
                return -1;
            } else if (value1 > value2) {
                return 1;
            } else {
                return 0;
            }
        };       
    }
```

