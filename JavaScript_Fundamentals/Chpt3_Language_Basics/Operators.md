# Operators

## Unary Operators
Operators that work on only one value are called *unary operators*.

### Increment/Decrement
There are two ways to operate increment or decrement:
* prefix

```javascript
    var age = 21;
    ++age;  // age = 22
```

* postfix

```java
    var age = 21;
    age = age + 1;  // age = 22
    
    var age = 21;
    age++;  // does the same work
```

When using prefix, the variable's value is changed before the statement is evaluated:

```javascript
    var age = 21;
    var anotherAge = --age + 2;
    
    alert(age); // outputs 20
    alert(anotherAge);  // outputs 22
```

When using postfix, the variable's value is not changed until the statement is evaluated:

```javascript
    var num1 = 2;
    var num2 = 20;
    var num3 = num1-- + num2; // equals 22
    var num4 = num1 + num2; // equals 21
```
Some rules:

```javascript
    var s1 = "2";
    var s2 = "z";
    var b = false;
    var f = 1.1;
    var o = {
      valueOf: function() {
        return -1;
      }
    };
    
    s1++; // numeric 3
    s2++; // NaN
    b++;  // numeric 1
    f--;  // 0.10000000000000009
    o--;  // -2
```

### Unary Plus and Minus
A single + sign:

```javascript
    var num1 = 25;
    num1 = +25; // num1 equals 25
    
    var num2 = 25;
    num2 += 25; // num2 equals 50
    
    var num3 = 25;
    num3 += num3; // num3 equals 50
```

When applied to a nonnumeric value, unary plus(or minus) performs the same conversion as the `Number()` casting function.

## Bitwise Operators
Values are converted from 64-bit to 32-bit integer, to perform bitwise opertaions, then the result is converted back to 64 bits. <br />
<br />
From right to left, 31 bits are used to represent the numeric value of integer. The 32nd bit, called *sign bit*, represents the sign of the number: 0 for positive and 1 for negative.

**There will be a separate note for the bitwise manipulation**

## Boolean Operators

### Logical NOT
Using exclamation `!`
```javascript
    
    alert(!false);  // true
    alert(!"blue"); // false
    alert(!0);  // true
    alert(!""); // true
    alert(!NaN);    // true
    alert(!12345);  // false
    alert(!null);   // true
    alert(!undefined);  // true
```
By using two **NOT** operators, we can simulate the behavior of the `Boolean()` casting function.

### Logical AND
Using double amperand `&&`
* It follows these rules:
    * If the first operand is an object, then the second operand is always returned.
    * If the second operand is an object, then the object is returned only if the first operand evaluates to `true`
    * If both operands are objects, then the second operand is returned.
    * If either operand is `null`, `NaN` or `undefined`, then `null`, `NaN` or `undefined` is returned respectively.

**The logical AND operator is a short-circuited operation: If the first operand determines the result, the second will never evaluated.**

```javascript
    var married = false;
    var result = (married && someUndeclaredVariable);   // error occurs here
    alert(result);  // this line will never executes
    
    var married = true;
    var result = (married && someUndeclaredVariable);   // no error
    alert(result);  // works
```

### Logical OR
Using double pipe `||`
* It follows these rules:
    * If the first operand is an object, then the first operand is returned.
    * If the first operand evaluates to `false`, then the second operand is returned.
    * If both operands are objects, then the first operand is returned.
    * If both operands are `null`, `NaN` or `undefined`, then `null`, `NaN` or `undefined` is returned respectively.
    
**The logical OR is also a short-circuited operation.**

```javascript
    var married = true;
    var result = (married || someUndeclaredVariable);
    alert(result);  // works
    
    var married = false;
    var result = (married || undeclaredVariable);   // error occurs here
    alert(result);  //  this line will never executes
```

## Multiplicative Operators
There are three multiplicative operators: multiply(`*`), divide(`/`) and modulus(`%`).<br />
<br />
I will simply skip this part, 
because these operators work in a manner similar to their counterparts in languages such as Java, C and Perl.

## Additive Operators
* If two operands are numbers, they perform an arithmetic add.
* If one of the operands is a string, then the following rules apply:
    * If both operands are strings, the second string is concatenated to the first.
    * If only one operand is a string, the other operand is converted to a string, then performs the concatenation.
* If either oerpand is an object, number, of Boolean, its `toString()` method is called to get a string value and then the previous rules regarding strings are applied.
* For `undefined` and `null`, the `String()` function is called to retrieve the values "undefined" and "null", respectively.

```javascript
    var result1 = 5 + 5;
    alert(result1); // 10
    var result2 = 5 + "5";
    alert(result2); // "55";
```
Consider the following two examples:

```javascript
    var num1 = 5;
    var num2 = 10;
    var result = "The sum of num1 and num2 is: " + num1 + num2;
    alert(result);  // the sum will be "510"
```

```javascript
    var num1 = 5;
    var num2 = 10;
    var result = "The sum of num1 and num2 is: " + (num1 + num2);
    alert(result);  // the sum will be "15";
```

I'm not gonna include subtract operator here.

## Relational Operators
As with other operators, there are some conversions and other oddities that happen when using different data types:
* If the operands are numbers, perform a numeric comparison.
* If the operands are strings, compare the character codes of each corresponding character in the string.
* If one operand is a number, convert the other operand to a number and perform a numeric comparison.
* If an operand is an object, call `valueOf()` and use its result to perform the comparison according to the previous rules. If `valueOf()` is not available, call `toString()` and use the value according to the previous rules.
* If an operand is a Boolean, convert it to a number and perform the comparison.

Some examples:

```javascript
    var result = "Brick" < "alphabet";  // true
    var result = "Brick".toLowerCase() < "alphabet".toLowerCase();  // false
    var result = "23" < "3";    // true
    var result = "23" < 3;  // false
    var result = "a" < 3;   // false because "a" becomes NaN
    var result1 = NaN < 3;  // false
    var result2 = NaN >= 3; // false
```

## Equality Operators
For ECMAScript, there are two sets of operators:
* *equal* and *not equal* to perform conversion before comparison.
* *identically equal* and *not identically equal* to perform comparison without conversion.

### Equal and Not Equal
`==` and `!=` <br />
Both operators do conversions to determine if two operands are equal(often called *type coercion*)
* Boolean value -> numeric value:
    * `false` -> 0
    * `true` -> 1
* String -> number if one string + one number
* call `valueOf()` method on the object if an object + a non-object
* values of `null` and `undefined` are equal and cannot be converted into any other values
* either operand is `NaN`, the `==` returns `false` and `!=` returns `true`
* If both operands are objects, then check if both operands point to the same object.

|**EXPRESSION**|**VALUE**|
|---|---|
|null == undefined|true|
|"NaN" == NaN|false|
|5 == NaN|false|
|NaN == NaN|false|
|NaN != NaN|true|
|false == 0|true|
|true == 1|true|
|true == 2|false|
|undefined == 0|false|
|null == 0|false|
|"5" == 5|true|

### Identically Equal and Not Identically Equal
`===` and `!==`

```javascript
    var result1 = ("55" == 55); // true after conversion
    var result2 = ("55" === 55);    // false without conversion
```

```javascript
    var result1 = ("55" != 55); // false after conversion
    var result2 = ("55" !== 55);    // true without conversion
```
**`null` == `undefined` is `true`, `null` === `undefined` is `false`**

## Conditional Operators
Syntax:

```javascript
    variable = boolean_expression ? true_value : false_value;
    var max = (num1 > num2) ? num1 : num2;
```

## Assignment Operators
`=`

```javascript
    var num = 10;
    
    // below two work the same
    num = num + 10;
    num += 10;  // this is called compound-assignment
```
* Multiply/assign `*=`
* Divide/assign `/=`
* Modulus/assign`%=`
* Add/assign`+=`
* Subtract/assign`-=`
* Left shift/assign`<<=`
* Signed right shift/assign`>>=`
* Unsigned right shift/assign`>>>=`

## Comma Operator

```javascript
    var num1 = 1, num2 = 2, num3 = 3;
    var num = (5, 1, 2, 6, 0);  // num becomes 0
```
