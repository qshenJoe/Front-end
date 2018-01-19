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

**The logical AND operator is a short-circuited operation: If the first operand determines the result, the second will never evaluated.**

```javascript
    var married = false;
    var result = (married && someUndeclaredVariable);   // error occurs here
    alert(result);  // this line will never executes
    
    var married = true;
    var result = (married && someUndeclaredVariable);   // no error
    alert(result);  // works
```

## Multiplicative Operators

## Additive Operators

## Relational Operators

## Equality Operators

## Conditional Operators

## Assignment Operators

## Comma Operator
