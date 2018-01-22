# Singleton Built-in Objects
This kind of objects are defined to be built-in object as "any object supplied by an ECMAScript implementation, independent of the host environment, which is present at the start of the execution of an ECMAScript program."
<br />
This object is already instantiated. Most of the built-in objects you've known are `Object`, `Array` and `String`. There are two singleton built-in objects defined by ECMA-262: `Global` and `Math`:

## The Global Object
It is not explicitly accessible.
* All variables and function defined globally become properties of the `Global` object.
* Functions such as `isNaN()`, `isFinite()`, `parseInt()`, and `parseFloat()` are actually methods of the `Global` object.

### URI-Encoding Method
* `encodeURI()` is designed to work on an entire URI(www.wrox.com/illegal value.htm)
* `encodeURIComponent()` is designed to work solely on a segment of a URI(illegal value.htm from the previous URI)
* Both of them are used to encode URIs(Uniform Resource Iedntifiers) to be passed to the browser.
* To be valid, a URL cannot contain certain characters, such as spaces.
* Invalid characters will be replaced with a special UTF-8 encoding.
* The main difference between the two methods is that `encodeURI()` does not encode special characters that are part of a URI, such as a colon, forward slash, question mark, and pound sign, whereas `encodeURIComponent()` encodes every nonstandard character it finds.

```javascript
    var uri = "http://www.wrox.com/illegal value.htm#start";
    
    //"http://www.wrox.com/illegal%20value.htm#start"
    alert(encodeURI(uri));  // using this method left the value completely intact except for the space, which was replaced with %20
    
    //"http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.htm%23start"
    alert(encodeURIComponent(uri));   // this method replaces all nonalphanumeric characters with their encoded equivalents
    
    // generally, you will use encodeURIComponent() much more frequently
```

As for the `decodeURI()` and `decodeURIComponent()`:

```javascript
    var uri = "http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.htm%23start";
    
    //http%3A%2F%2Fwww.wrox.com%2Fillegal value.htm%23start
    alert(decodeURI(uri));
    
    //http://www.wrox.com/illegal value.htm#start
    alert(decodeURIComponent(uri));
```

### The eval() Method
This method works like an entire ECMAScript intrepreter and accepts one argument, a string of ECMAScript(or JavaScript) to execute.

```javascript
    eval("alert('hi')");  // this line is functionally equivalent to the following
    
    alert("hi");
    
    /* 
     * Code executed by eval() is considered to be part of the execution context in which the call is made,
     * and the executed code has the same scope chain as that context.
     * This means variables that are defined in the containing context
     * can be referenced inside an eval() call
     */
     
     var msg = "hello world";
     eval(alert(msg));  // "hello world"
     
     eval("function sayHi() { alert('hi'); }");
     sayHi();
     
     eval("var msg = 'hello world';");
     alert(msg);    // "hello world"
     
     // the last two examples will cause error when strict mode is used
     "use strict";
     eval = "hi";   // causes error
```

### Global Object Properties
Properties of `Global` object:

|**PROPERTY**|**DESCRIPTION**|
|---|---|
|`undefined`|The special value `undefined`|
|`NaN`|-|
|`Infinity`|-|
|`Object`|Constructor for `Object`|
|`Array`|-|
|`Function`|-|
|`Boolean`|-|
|`String`|-|
|`Number`|-|
|`Date`|-|
|`RegExp`|-|
|`Error`|-|
|`EvalError`|-|
|`RangeError`|-|
|`ReferenceError`|-|
|`SyntaxError`|-|
|`TypeError`|-|
|`URIError`|-|

**In ECMAScirpt 5, it's explicitly disallowed to assign values to `undefined`, `NaN`, and `Infinity`. Doing so causes an error even in nonstrict mode.**

### The Window Object
The `window` is the Global object's delegate.

```javascript
    var color = "red";
    function sayColor() {
        alert(window.color);
    }
    
    window.sayColor();    // "red"
    
    // another way to retrieve the Global object is to use:
    
    var global = function() {
        return this;
    }();
    
    /*
     * This code creates an immediately-invoked function expression
     * that returns the value of this.
```

## The Math Object

### Math Object Properties

|**PROPERTY**|**DESCRIPTION**|
|---|---|
|`Math.E`|The value of e, the base of the natural logarithms|
|`Math.LN10`|The natural logarithm of 10|
|`Math.LN2`|The natural logarithm of 2|
|`Math.LOG2E`|The base 2 logarithm of e|
|`Math.LOG10E`|The base 10 logarithm of e|
|`Math.PI`|The value of PI|
|`Math.SQRT1_2`|The square root of 1/2|
|`Math.SQRT2`|The square root of 2|

### The min() and max() Methods
These two methods determine which number is the smallest or largest in a group of numbers. They accept any number of parameters.

```javascript
    var max = Math.max(3, 54, 32, 16);
    alert(max);   // 54
    
    var min = Math.min(3, 54, 32, 16);
    alert(min);   // 3
    
    // To find the maximum or the minimum value in an array, use apply() method
    
    var values = [1, 2, 3, 4, 5, 6, 7, 8];
    var max = Math.max.apply(Math, values);
    
    // The key is to pass in the Math object as the first argument of apply() so that the this value is set appropriately
```

### Rounding Methods
These methods have to do with rounding decimal values into integers.
* `Math.ceil()` - rounds numbers up to the nearest integer value
* `Math.floor()` - rounds numbers down to the nearest integer value
* `Math.round()` - rounds up if the number is at least halfway to the next integer value (0.5 or higher) and rounds down if not

```javascript
    alert(Math.ceil(25.9));   // 26
    alert(Math.ceil(25.5));   // 26
    alert(Math.ceil(25.1));   // 26
    
    alert(Math.round(25.9));    // 26
    alert(Math.round(25.5));    // 26
    alert(Math.round(25.1));    // 25
    
    alert(Math.floor(25.9));    // 25
    alert(Math.floor(25.5));    // 25
    alert(Math.floor(25.1));    // 25
```

### The random() Methods
The `Math.random()` method returns a random number between the 0 and the 1, not including either 0 or 1

```javascript
    number = Math.floor(Math.random() * total_number_of_choice + first_possible_value);
    
    // select a number between 1 and 10
    var num = Math.floor(Math.random() * 10 + 1);
    
    // select a number between 2 and 10
    var num = Math.floor(Math.random() * 9 + 2);
    
    // Easier to use a function
    function selectFrom(lowerValue, upperValue) {
        var choices = upperValue - lowerValue + 1;
        return Math.floor(Math.random() * choices + lowerValue);
    }
    
    var num = selectFrom(2, 10);
    alert(num);   // number between 2 and 10, inclusive
    
    // select a random item from an array 
    var colors = ["red", "green", "blue", "yellow", "black", "purple", "brown"];
    var color = colors[selectFrom(0, colors.length - 1)];
```

### Other Methods

|**METHOD**|**DESCRIPTION**|
|---|---|
|`Math.abs(num)`|Returns the absolute value of `(num)`|
|`Math.exp(num)`|Returns `Math.E` raised to the power of `(num)`|
|`Math.log(num)`|Returns the natural logarithm of `(num)`|
|`Math.pow(num, power)`|Returns num raised to the power of `power`|
|`Math.sqrt(num)`|Returns the square root of `(num)`|
|`Math.acos(x)`|Returns the arc cosine of *x*|
|`Math.asin(x)`|Returns the arc sine of *x*|
|`Math.atan(x)`|Returns the arc tangent of *x*|
|`Math.atan2(y,x)`|Returns the arc tangent of *y/x*|
|`Math.cos(x)`|Returns the cosine of *x*|
|`Math.sin(x)`|Returns the sine of *x*|
|`Math.tan(x)`|Returns the tangent of *x*|
