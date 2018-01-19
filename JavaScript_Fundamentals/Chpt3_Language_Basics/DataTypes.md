# Data Types
We are dealing with different types of data every day. <br />
* Five primitive data types:
  * undefined
  * null
  * number
  * string
  * boolean
* One complex data type:
  * object

### Usage of `typeof` operator
The `typeof` operator can be called like:

```javascript
    var name,
        age = 21;
    alert(typeof name); // undefined
    alert(typeof age); // number
    alert(typeof 21); // number
    alert(typeof "21"); // string
    alert(typeof true); // boolean
```

Some confusing cases:

```javascript
    alert(typeof null); // object, as the value null is considered to be an empty object reference
    alert(typeof function(){}); // function - Chrome
```

## The Undefined Type
When a variable is declared using `var` without initilizing, or assigned a value explicitly,
it will be implicitly assigned a value of `undefined`. <br />
<br />
**Basically, you should never explicitly declare a variable with value of `undefined`.**<br />
<br />
Note the difference between the undeclared variable and the uninitilized variable:

```javascript
    var age;
    
    // Here is another variable undeclared
    // var name
    
    alert(age); // undefined
    alert(name); // causes an error
    
    alert(typeof age); // undefined
    alert(typeof name); // undefined 
```

## The Null Type
The null data type has only one value `null`.<br />
Logically, a `null` value is an empty object pointer.

```javascript
    var myObject = null;
    alert(typeof myObject); // object
```
    
It is suggested to defined a variable, which is meant to later hold an object, with the value of `null`.

```javascript
    if(myObject != null) {
       // do something with myObject
    }
```

The value `undefined` is a derivative of `null`.

```javascript
    alert(undefined == null); // true
```
## The Boolean Type
Two literal values: `true` and `false`.

```javascript
    var married = false;
    var graduated = true;
```

Type conversion:

```javascript
    var age = 21;
    alert(age); // 21

    var ageAsBoolean = Boolean(age);
    alert(ageAsBoolean); // true
```

|**DATA TYPE**|**VALUES CONVERTED TO TRUE**|**VALUES CONVERTED TO FALSE**|
|-----|-----|-----|
|Boolean|`true`|`false`|
|String|Any non-empty string|""(empty string)|
|Number|Any non-zero number(including infinity)|`0`,`NaN`|
|Object|Any object|`null`|
|Undefined|n/a|`undefined`|

Understanding the above table is important, because some flow control statements such as the `if` statement will automatically perform the Boolean conversion.

```javascript
    var age = 21;
    if (age) {
        alert("Variable defined");
    }
```

## The Number Type
* Integer formats
 * Decimal integer
 * Octal(base 8) integer
 * hexadecimal(base 16) integer
Here are some examples:

```javascript
    var intNum = 15; // integer
    var octalNum1 = 070; // octal for 56
    var octalNum2 = 079; // invalid octal - interpreted as 79
    var octalNum3 = 08; // invalid octal - interpreted as 8
    
    var hexNum1 = 0xA; // hexadecimal for 10
    var hexNum2 = 0x1f; // hexadecimal for 31
```
**Octal and hexadecimal numbers are treated as decimal numbers in all arithmetic operations.**

* Floating-point values

```javascript
    var floatNum1 = 1.1;
    var floatNum2 = 0.1;
    var floatNum3 = .1; // valid but not recommended
```

ECMAScript always looks for ways to convert floating-point values into integers in such cases to save memory:

```javascript
    var floatNum1 = 1.; // missing digit after decimal - interpreted as integer 1
    var floatNum2 = 10.0; // whole number - interpreted as integer 10
```

Use *e-notation* to represent large floating-point values:

```javascript
    var floatNum = 3.125e7; // equal to 31250000
```

**Rounding errors**:
```javascript
    if (a + b == 0.3){
        alert("You got 0.3!");
    }
    /*
     * We should avoid testing specific floating-point values
     * Because 0.1 + 0.2 yields 0.30000000000000004
     */
```
**Range of values**
* Numbers smaller than Number.MIN_VALUE are represented as `-Infinity`
* Smallest number is stored in Number.MIN_VALUE and is 5e-324
* Largest number is stored in Number.MAX_VALUE and is 1.7976931348623157e+308
* Numbers larger than Number.MAX_VALUE are represented as `Infinity`

There is the `isFinite()` function to check whether a number is finite.

```javascript
    var result = Number.MIN_VALUE + Number.MAX_VALUE;
    alert(isFinite(result)); // false
```

### NaN
`NaN` is short for Not a Number. <br />
In ECMAScript, dividing a number by 0 always returns `NaN`.
* Any operation involving `NaN` always returns `NaN`
* `NaN` is not equal to any value, including itself.

```javascript
    alert(NaN == NaN); // false
```

ECMAScript has provided isNaN() function to check whether a variable is of `NaN` value.

```javascript
    alert(isNaN(NaN)); // true
    alert(isNaN(10)); // false
    alert(isNaN("10")); // false, "10" can be converted to a number
    alert(isNaN("Hello")); // true, cannot be converted to a number
    alert(isNaN(true)); // false, can be converted to number 1
```

### Number conversions
Three functions to convert nonnumeric values into numbers:
* Number() - can be used on any data type
  * Boolean: `true` -> 1 and `false` -> 0
  * numbers
  * `null`: returns 0
  * `undefined`: returns `NaN`
  * strings:
    * "123" -> 123; "011" -> 11
    * "1.1" -> 1.1; "001.1" -> 1.1
    * "0xf" -> 15
    * "" -> 0
    * other formats -> `NaN`
  * objects: `valueOf()` and `toString()` methods are called accordingly.
* parseInt() - used to convert strings to numbers

```javascript
    var num1 = parseInt("1234blue"); // 1234
    var num2 = parseInt(""); // NaN
    var num3 = parseInt("0xA"); // 10 - hexadecimal
    var num4 = parseInt("22.5"); // 22
    var num5 = parseInt("70"); // 70
    var num6 = parseInt("0xf"); // 15 - hexadecimal
    var num7 = parseInt("070"); // 70
```
  * By using a second argument:
  
```javascript
    var num = parseInt("0xAF", 16); // 175
    var num1 = parseInt("AF", 16); // 175
    var num2 = parseInt("AF"); // NaN
```

```javascript
    var num1 = parseInt("10", 2); // 2 - parsed as binary
    var num2 = parseInt("10", 8); // 8 - parsed as octal
    var num3 = parseInt("10", 10); // 10 - parsed as decimal
    var num4 = parseInt("10", 16); // 16 - parsed as hexadecimal
```

* parseFloat() - used to convert strings to numbers

```javascript
    var num1 = parseFloat("1234blue"); // 1234 - integer
    var num2 = parseFloat("0xA"); // 0
    var num3 = parseFloat("22.5"); // 22.5
    var num4 = parseFloat("22.34.5"); // 22.34
    var num5 = parseFloat("0908.5"); // 908.5
    var num6 = parseFloat("3.125e7"); // 32150000
```

## The String Type
Both double quotes(") and single quotes(') are legal.

```javascript
    var firstName = "Joe";
    var lastName = 'Shen';
    var gender = 'Male"; // syntax error - quotes must match
```
### Character literals
|**LITERAL**|**MEANING**|
|-----|-----|
|`\n`|New line|
|`\t`|Tab|
|`\b`|Backspace|
|`\r`|Carriage return|
|`\f`|Form feed|
|`\\`|Backslash(\)|
|`\'`|Single quote|
|`\"`|Double quote|
|`\xnn`|Hexadecimal code|
|`\unnnn`|Unicode character|

These character literals will be interpreted like a single character.

```javascript
    var text = "This is how A represented with hexadecimal code: \x41";
    alert(text.length); // outputs 50
```
However, this property returns the number of 16-bit characters in the string.
If a string contains double-byte characters, the `length` property may not accurately return the number of characters in the string.

**The nature of Strings**
Strings are immutable in ECMAScript.

### Converting to a string
Two possible ways:
* `toString()` method

```javascript
    var age = 21;
    var ageAsString = age.toString(); // "21"
    var married = false;
    var marriedAsString = married.toString(); // "false"
    
    /* tostring() applies to numbers,
     * Booleans, objects, and strings.
     * If a value is null or undefined,
     * the method is not available.
     */
```

When used on a number value, `toString()` takes a single argument:

```javascript
    var num = 10;
    alert(num.toString()); // "10"
    alert(num.toString(2)); // "1010"
    alert(num.toString(8)); // "12"
    alert(num.toString(10)); // "10"
    alert(num.toString(16)); // "a"
```

* `String()` casting function
  * If a value has a `toString()` method, it is called and the result is returned.
  * If a value is `null`, "null" is returned.
  * If a value is `undefined`, "undefined" is returned.
  
```javascript
    var value1 = 10;
    var value2 = true;
    var value3 = null;
    var value4;
    
    alert(String(value1)); // "10"
    alert(String(value2)); // "true"
    alert(String(value3)); // "null"
    alert(String(value4)); // "undefined"
```

## The Object Type
We can create our own objects by creating instances of the `Object` type and adding properties and/or methods to it.

```javascript
    var myObject = new Object();
```

Each `Object` instance has the following properties and methods:
* `constructor` - the `Object()` function
* `hasOwnProperty(propertyName)` - indicates if the given property exists on the object instance(not on the prototype). The propertyName must be a string.
* `isPrototypeOf(object)`
* `propertyIsEnumerable(propertyName)` - indicates if the given property can be enumerated using the `for-in` statement.
* `toLocaleString()`
* `toString()`
* `valueOf()`
