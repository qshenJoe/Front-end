# The Function Type
Each function is an instance of the `Function` type that has properties and methods just like any other reference type.
* function names are simply pointers to function objects
<br />
function-declaration syntax:

```javascript
    function sum(num1, num2) {
        return num1 + num2;
    }
    
    // that is almost exactly equivalent to using function expression:
    
    var sum = function(num1, num2) {  // no name included after the function keyword
        return num1 + num2;   // the function can be referenced by the variable sum
    };
    
    // define functions using the Function constructor, which accepts any number of arguments and the last argument is always considered to be the function body
    
    var sum = new Function("num1", "num2", "return num1 + num2");   // not recommended
```

It's possible to have multiple names for a single function, because function names are simply pointers to functions.

```javascript
    function sum(num1, num2) {
        return num1 + num2;
    }
    alert(sum(10,10));    // 20
    
    var anotherSum = sum;   // using the function name without parentheses accesses the function pointer instead of executing the function
    alert(anotherSum(10,10));   // 20
    
    sum = null;   // This set the variable sum to point to null
    alert(anotherSum(10,10));   // 20 because anotherSum still points to the created function object
```

## No Overloading(Revisited)
Thinking of function names as pointers also explains why there can be no function overloading in ECMAScirpt.

```javascript
    function addSomeNum(num) {
        return num + 100;
    }
    
    function addSomeNum(num) {
        return num + 200;
    }
    
    var result = addSomeNum(100);   // 300
    
    // Same as the following
    var addSomeNum = function (num) {
        return num + 100;
    };
    addSomeNum = function (num) {   // the variable addSomeNum is simply being overwritten when the second function is created
        return num + 200;
    };
    
    var result = addSomeNum(100);   // 300
```

## Function Declarations vesus Function Expressions
* Function declarations are read and available in an execution context before any code is executed.
* Function expressions aren't complete until the execution reaches that line of code.

```javascript
    alert(sum(10,10));
    function sum(num1, num2) {
        return num1 + num2;
    }
    
    /*
     * function declarations are read and added to 
     * the execution context before the code begins
     * running through a process called
     * function declaration hoisting
     * As the code is being evaluated, the JavaScript
     * engine does a first pass for function declarations
     * and pulls them to the top of the source tree
     */
     
     alert(sum(10,10));
     var sum = function(num1, num2) {   // causes an error during execution
        return num1 + num2;
     };
     
     /*
      * the function is part of an initialization statement,
      * not part of a function declaration. That means
      * the function is not available in the variable sum
      * until the second line has been executed, which won't
      * happen, because the first line causes an 
      * "unexpected identifier" error.
      */
```

**It is possible to have named function expressions that look like declarartions, such as var sum = function sum() {}.**

## Function as Values
Function names in ECMAScript are nothing more than variables, so functions can be used any place any other value can be used.
* You can pass a function into another function as an argument
* You can also return a function as the result of another function

```javascript
    function callSomeFunction(someFunction, someArgument) {
        return someFunction(someArgument);
    }
    
    // any function can then be passed in as follows:
    function add10(num) {
        return num + 10;
    }
    
    var result1 = callSomeFunction(add10,10);
    alert(result1);   // 20
    
    function getGreeting(name) {
        return "Hello, " + name;
    }
    
    var result2 = callSomeFunction(getGreeting, "Joe");
    alert(result2);   // "Hello, Joe";
    
    /*
     * The callSomeFunction() function is generic
```

Returning a function from a function can be quite useful:

```javascript
    function createComparisonFunction(propertyName) {
        return function(object1, object2) {
            var value1 = object1[propertyName];
            var value2 = object2[propertyName];
            
            return value1 - value2;
        };
    }
    
    var data = [{name: "Joe", age: 21}, {name: "Qiaohong", age: 18}];
    data.sort(createComparisonFunction("name"));
    alert(data[0].name);    // Joe
    
    data.sort(createComparisonFunction("age"));
    alert(data[0].name);    // Qiaohong
    
    /*
     * By default, the sort() method would call
     * toString() on each object to determine the
     * sort order, which won't give logical results
     * in this case.
     * Calling createComparisonFunction("name") creates
     * a comparison function that sorts based on the name 
     * property, which means the first item will have 
     * the name "Joe" and an age of 21.
     */
```

## Function Internals
Two special objects exist inside a function:
* `arguments`
  * the `arguments` object has a property named `callee`, which is a pointer to the function that owns the `arguments` object

```javascript
    function factorial(num) {
        if (num <= 1) {
            return 1;
        } else {
            return num * factorial(num - 1);
        }
    }
    
    /*
     * Factorial functions are typically defined
     * to be recursive.
     * In this example, the proper execution of this 
     * function is tightly coupled with the function
     * name "factorial". It can be decoupled by using
     * arguments.callee as follows:
     */
     
     function factorial(num) {
        if (num <= 1) {
            return 1;
        } else {
            return num * arguments.callee(num - 1);
        }
     }
     
     // there is no longer a reference to the name "factorial" in the function body
     var trueFactorial = factorial;
     
     factorial = function() {
        return 0;
     };
     alert(trueFactorial(5));   // 120
     alert(factorial(5));   // 0
     
     // Without using arguments.callee in the original factorial() function's body, the call to trueFactorial() would return 0
```

* `this`
  * It is a reference to the context object that the function is operating on - often called the *this value*
  * When a function is called in the global scope of a web page, the `this` object points to `window`

```javascript
    window.color = "red";
    var o = { color: "blue" };
    
    function sayColor() {
        alert(this.color);
    }
    
    sayColor();   // "red"  the execution context is global - window
    
    o.sayColor = sayColor;
    o.sayColor();   // "blue" the execution context is inside the object o
```

ECMAScript 5 also formalizes an additional property on a function object: `caller`
* contains a reference to the function that called this function or `null` if the function was called from the global scope

```javascript
    function outer() {
        inner();
    }
    
    function inner() {
        alert(inner.caller);
    }
    
    outer();
    
    // for looser coupling, you can use arguments.callee.caller
    function outer() {
        inner();
    }
    function inner() {
        alert(arguments.callee.caller);
    }
    outer();
```

**When function code executes in strict mode, attempting to access `argument.callee` results in an error.**
<br />
ECMAScript 5 also defines `arguments.caller`, which also results in an error in strict mode and is always undefined outside the strict mode.
* This is to clear up confusion between `arguments.caller` and the `caller` property of functions
* Strict mode places one additional restriction: you cannot assign a value to the caller property of a function. Doing so results in an error.

## Function Properties and Methods
Each function has two properties: `length` and `prototype`
* The `length` indicates the number of named arguments that the function expects

```javascript
    function sayName(name) {
        alert(name);
    }
    
    function sum(num1, num2) {
        return num1 + num2;
    }
    
    function sayHi() {
        alert("Hi");
    }
    
    alert(sayName.length);    // 1
    alert(sum.length);    // 2
    alert(sayHi.length);    // 0
```

* The `prototype` property is the actual location of all instance methods for reference types, meaning methods such as `toString()` and `valueOf()` actually exist on the `prototype` and are then accessed from the object instances.
  * In ECMAScript 5, the `prototype` property is not enumerable and so will not be found using `for-in`
<br />
There are two additional methods for functions: `apply()` and `call()`
* These methods both call the function with a specific `this` value, setting the value of the `this` object inside the function body.
* `apply()` method accepts two arguments: the value of `this` inside the function and an array of arguments
  * This second argument may be an instance of `Array`, but it can also be the `arguments` object

```javascript
    function sum(num1, num2) {
        return num1 + num2;
    }
    
    function callSum1(num1, num2) {
        return sum.apply(this, arguments);    // passing in arguments object
    }
    
    function callSum2(num1, num2) {
        return sum.apply(this, [num1, num2]);   // passing in array
    }
    
    alert(callSum1(10,10));   // 20
    alert(callSum2(10,10));   // 20
    
    /*
     * In strict mode, the this value of a function
     * called without a context object is not
     * coerced to window.
     * Instead, this becomes undefined unless
     * explicitly set by either attaching the function
     * to an object or using apply() or call().
     */
```

* The `call()` method exhibits the same behavior as `apply()`, but arguments are passed to it differently.
  * The first argument is the `this` value
  * The remaining arguments are passed directly into the function
  * Using `call()` arguments must be enumerated specifically
  
```javascript
    function sum(num1, num2) {
        return num1 + num2;
    }
    
    function callSum(num1, num2) {
        return sum.call(this, num1, num2);
    }
    
    alert(callSum(10,10));    // 20
```

The true power of `apply()` and `call()` lies not in their ability to pass arguments but rather in their ability to augment the `this` value inside of the function.

```javascript
    window.color = "red";
    var o = { color: "blue" };
    
    function sayColor() {
        alert(this.color);
    }
    
    sayColor();   // red
    sayColor.call(this);    // red
    sayColor.call(window);    // red
    sayColor.call(o);   // blue   switch the context of the function such that this points to o
```

ECMAScript 5 defines an additonal method called `bind()`. The `bind()` method creates a new function instance whose `this` value is *bound* to the value that was passed into `bind()`.

```javascript
    window.color = "red";
    var o = { color: "blue" };
    
    function sayColor() {
        alert(this.color);
    }
    var objectSayColor = sayColor.bind(o);    // The objectSayColor() function has a this value equivalent to o
    objectSayColor();   // blue
```
