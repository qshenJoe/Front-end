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

## The String Type

## The Object Type
    
