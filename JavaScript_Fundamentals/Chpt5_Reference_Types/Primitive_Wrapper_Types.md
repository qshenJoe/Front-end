# Primitive Wrapper Types
Three reference types are designed to ease interaction with primitive values:
* the `Boolean` type
* the `Number` type
* the `String` type

<br />
Every time a primitive value is read, an object of the corresponding primitive wrapper type is created behind the scenes, allowing access to any number of methods for manipulating the data.

```javascript
    var s1 = "some text";   // s1 is a primitive value
    var s2 = s1.substring(2);
    
    /* 
     * When s1 is accessed in the second line, it is 
     * being accessed in read mode, which is to say its
     * value is being read from memory.
     * Any time a string value is accessed in read mode,
     * the following three steps occur:
     * 1. Create an instance of the String type
     * 2. Call the specified method on the instance
     * 3. Destroy the instance
     */
     
     // You can think of these three steps as they're used in the following three lines of ECMAScript code:
     
     var s1 = new String("some text");
     var s2 = s1.substring(2);
     s1 = null;
```

The major difference between reference types and primitive wrapper types is the lifetime of the object
* when you instantiate a reference type using the `new` operator, it stays in memory until it goes out of scope
* automatically created primitive wrapper objects exist for only one line of code before they are destroyed
* this means that properties and methods cannot be added at runtime:

```javascript
    var s1 = "some text";
    s1.color = "red";
    alert(s1.color);    // undefined
    
    // the third line creates its own String object, which doesn't have the color property
```

Calling `typeof` on an instance of a primitive wrapper type returns "object", and all primitive wrapper objects convert to the Boolean value `true`.
<br />
The `Object` constructor also acts as a factory method and is capable of returning an instance of a primitive wrapper based on the type of value passed into the constructor.

```javascript
    var obj = new Object("some text");
    alert(obj instanceof String);   // true
    
    /*
     * a string argument results in an instance of String
     * a number argument results in an instance of Number
     * a Boolean argument results in an instance of Boolean
     */
```

Calling a `primitive wrapper` constructor using `new` is not the same as calling the casting function of the same name:

```javascript
    var value = "25";
    var number = Number(value);   // casting function
    alert(typeof number);
    
    var obj = new Number(value);    // constructor
    alert(typeof obj);    // "object"
    
    /* 
     * the variable number is filled with a primitive number value
     * the variable obj is filled with an instance of Number
```

## The Boolean Type
To create a `Boolean` object, use the `Boolean` constructor and pass in either `true` or `false`:

```javascript
    var booleanObject = new Boolean(true);
```

* Instances of `Boolean` override the `valueOf()` method to return a primitive value of either `true` or `false`.
* The `toString()` method is also overridden to return a string of "true" or "false" when called.

<br />
However problem occurs when trying to use `Boolean` objects in Boolean expressions:

```javascript
    var falseObject = new Boolean(false);
    var result = falseObject && true;   // it is the object falseObject being evaluated, not its value(false)
    alert(result);    // true
    
    var falseValue = false;
    var result = falseValue && true;
    alert(result);    // false
```

Differences between the primitive and the reference Boolean types
* The `typeof` operator returns "boolean" for the primitive but "object" for the reference.
* A `Boolean` object is an instance of the `Boolean` type and will return `true` when used with `instanceof` operator, whereas a primitive value returns `false`

```javascript
    alert(typeof falseObject);    // object
    alert(typeof falseValue);   // boolean
    alert(falseObject instanceof Boolean);    // true
    alert(falseValue instanceof Boolean);   // false
```

## The Number Type
To create a `Number` object:

```javascript
    var numberObject = new Number(10);
```

As with the `Boolean` type, the `Number` type overrides `valueOf()`, `toLocaleString()`, and `toString()`.
* The `valueOf()` method returns the primitive numeric value represented by the object
* The other two methods return the number as a string

```javascript
    var num = 10;
    alert(num.toString());    // "10"
    alert(num.toString(2));   // "1010"
    alert(num.toString(8));   // "12"
    alert(num.toString(10));    // "10"
    alert(num.toString(16));    // "a"
```

The `Number` type has several additional methods used to format numbers as strings.
* The `toFixed()` method returns a string representation of a number with a specified number of decimal points:
* The `toFixed()` method can represent numbers with 0 through 20 decimal places

```javascript
    var num = 10;
    alert(num.toFixed(2));    // "10.00"
    
    // The argument indicates how many decimal places should be displayed.
    
    // If the number has more than the given number of decimal places, the result is rounded to the nearest decimal place
    
    var num = 10.005;
    alert(num.toFixed(2));    // "10.01"
```

Another method to format numbers is the `toExponential()` method, which returns a string with the number formatted in exponential notation(aka e-notation).
* accepts one argument

```javascript
    var num = 10;
    alert(num.toExponential(1));    // "1.0e+1"
    
    // If you want to have the most appropriate form of small numbers, use toPrecision() instead
    
    var num = 99;
    alert(num.toPrecision(1));    // "1e+2"
    alert(num.toPrecision(2));    // "99"
    alert(num.toPrecision(3));    // "99.0"
    
    // this method takes one argument, which is the total number of digits to use to represent the number(not including exponents)
    // The toPrecision() method can represent numbers with 1 through 21 decimal places
```

`Number` objects should not be instantiated directly.

```javascript
    // The typeof and instanceof operators work differently when dealing with primitive numbers versus reference numbers
    
    var numberObject = new Number(10);
    var numberValue = 10;
    alert(typeof numberObject);   // "object"
    alert(typeof numberValue);    // "number"
    alert(numberObject instanceof Number);    // true
    alert(numerValue instanceof Number);    // false
```

## The String Type
The `String` type is the object representation for strings and is created using the `String` constructor:

```javascript
    var stringObject = new String("hello world");
```

The methods of a `String` object are available on all string primitives. All three of the inherited methods - `valueOf()`, `toLocaleString()`, and `toString()` - return the object's primitive string value.
* each instance of `String` contains a single property, `length`, which indicates the number of characters in the string

```javascript
    var stringValue = "hello world";
    alert(stringValue.length);    // "11"
```

### Character Methods
Two methods access specific characters in the string: `charAt()` and `charCodeAt()`
* each accpets a single argument, which is the character's zero-based position
* The `charAt()` method returns the character in the given position as a single-character string

```javascript
    var stringValue = "hello world";
    alert(stringValue.charAt(1));   // "e"
    
    var stringValue = "hello world";
    alert(stringValue.charCodeAt(1));   // outputs "101" which is the character code of "e"
    
    // another way to access an individual character
    var stringValue = "hello world";
    alert(stringValue[1]);    // "e"
```

### String-Manipulation Methods

### String Location Methods

### The trim() Method

### String Case Methods

### String Pattern-Matching Methods

### The localeCompare() Method

### The fromCharCode() Method

### HTML Methods
