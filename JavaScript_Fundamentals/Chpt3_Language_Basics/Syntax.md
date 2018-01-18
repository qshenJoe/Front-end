# SYNTAX of JavaScript
Learning the syntax of a programming language is learning its core. A wall cannot be built without bricks.
So, we will begin with the most basic things with JavaScript.

## Case-sensitivity
Everything in JavaScript, such as variable name, function name and operators. <br />
It is obvious:

    test and Test are different.  

## Identifiers
An identifier can be used to reference a variable, a function or a property. <br />
In JavaScript, identifiers must follow the following format.
> 1. The first character must be a letter, an underscore(_) or a dollar sign($).
> 2. All other characters must be letters, numbers, underscores or dollar signs.
> 3. Identifiers use camel case:
    
    myObject
    firstLetter
    doSomethingElse
    
**!Keywords, reserved words, true, false, and null cannot be used as identifiers**

## Comments
Comments can be very useful for not only you, but others to read you code. 
Having a good habit of styling your comment is important for readibility.
Here is two common styles of comments in JavaScript:
> For single line comment:
    
    // use two forward dash in the front of comments

> For multi-line comment:

    /*
     *  This is 
     *  how it works
     *  in a multi-line comment
     */

## Strict Mode
To enable the strict mode for the entire script, add the following statement:

    "use strict";
    
To enable the strict mode for a specific function, add the above statement inside a function:

    function() {
      "use strict";
      // function body
    }
    
## Statement
It is recommended always end a statement with a semicolon:

    var sum = a + b;
    var product = a * b;

Multiple statements are combined into a block using a pair of curly braces "{" and "}":

    if (age < 18) {
      alert("Not old enough!");
      age++;
    }
    
**It is preferred to always use code blocks with control statements such as *if*.**

## Variables
Variables in JavaScript are loosely typed, being able to hold any type of data.<br />
Follow the naming syntax:

    // var is an operator and age is an identifier.
    var age;
    /* With no value initialized or assigned to age,
     * the variable will hold a value - undefined
     */

Define the variable and set its value:

    var age = 21; // Here age is defined to hold a number value of 21

This does not necessarily mean that we cannot change the value of the variable,
we can even change the data type of it.

    var age = 21;
    age = "Old Enough"; // This is not recommended.
    
Using *var* to define a variable makes it local:

    function sayHi() {
      var message = "Hi!"; // local variable
    }
    sayHi();
    alert(message); // you will get error information

Defining a variable without *var* makes it global:
    
    function sayHi() {
      message = "Hi!"; // global variable
    }
    sayHi();
    alert(message); // "Hi!";

**It is not recommended to omit var to define global variables, which makes it hard to maintain code and might cause confusion.**

Define more variables using one *var*:

    var name = "Joe",
        age = 21,
        gender = "Male",
        married = false;

