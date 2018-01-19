# Statements
There are many flow-control statements.
## The `if` Statement
Syntax:

```javascript
    if (condition) statement1 else statement2
    
    if (age > 18) {
       alert("Old enough.");
    } else {
       alert("Not old enough.");
    } // It's considered best practice to always use block statements
```

You can chain `if` statements like this:

```javascript
    if (num > 10) {
       alert("Larget than 10.");
    } else if (num < 5) {
       alert("Smaller than 5.");
    } else {
       alert("Between 5 and 10 inclusive.");
    }
```

## The `do-while` Statement
This statement is a **post-test** loop, meaning that the code inside the loop will be executed at least once before the expression is evaluated.

```javascript
    do {
      statement
    } while (expression);
    
    var i = 0;
    do {
        i++;
    } while (i < 5);
```

## The `while` Statement
This statement is a **pretest** loop, which means the code inside the loop will be executed after the expression is evaluated.

```javascript
    while (expression) statement
    
    while (i < 5) {
        i++;
    }
```

## The `for` Statement
This statement is a **pretest** loop

```javascript
    for (initilization; expression; post-loop-expression) statement
    
    var count = 10;
    for (var i = 0; i < count; i++) {
        alert(i);
    }
    
    // the var keyword can be used outside the for loop
    var count = 10;
    var i;
    for (i = 0; i < count; i++) {
        alert(i);
    }
    
    // a variable declared inside a loop is accessible outside the loop
    var count = 10;
    for (var i = 0; i < count; i++) {
        alert(i);
    }
    alert(i);   // 10
```

The initilization, expression and postloop expression are all optional.

```javascript
    // this is an infinite loop
    for (;;) {
        doSomething();
    }
    
    // By including the control expression, we can turn a for loop into a while loop
    var count = 10;
    var i = 0;
    for (;i < count;) {
        alert(i);
        i++;
    }
```

## The `for-in` Statement
This statement is a strict iterative statement. It is used to enumerate the properties of an object.

```javascript
    for (property in expression) statement
    
    for (var propName in window) {
        document.write(propName);
    }
```

## Labeled Statements
We can label statements for later use:

```javascript
    label: statement
    
    start: for (var i = 0; i < count; i++) {
        alert(i);
    }
```

## The `break` and `continue` Statements
The `break` statement exits the loop immediately. Execution continues with the next statement after the loop.<br />
The `continue` statement exits the loop immediately, but execution continues from the top of the loop.

```javascript
    var num = 0;
    for (var i = 1; i < 10; i++) {
        if (i % 5 == 0) {
            break;
        }
        num++;
    }
    alert(num);   // 4
    
    var num = 0;
    for (var i = 1; i <  10; i++) {
        if (i % 5 == 0) {
            continue;
        }
        num++;
    }
    alert(num);   // 8
```

`break` with labeled statements:

```javascript
    var num = 0;
    outermost:
    for (var i = 0; i < 10; i++) {
        for (var j = 0; j < 10; j++) {
            if (i == 5 && j == 5) {
                break outermost;
            }
            num++;
        }
    }
    alert(num);   // 55
```

`continue` with labeled statements:

```javascript
    var num = 0;
    outermost:
    for (var i = 0; i < 10; i++) {
        for (var j = 0; j < 10; j++) {
            if (i == 5 && j == 5) {
                continue outermost;
            }
            num++;
        }
    }
    alert(num);   // 95
```

## The `with` Statement
The `with` statement sets the scope of the code within a particular object:

```javascript
    with (expression) statement;
    
    var qs = location.search.substring(1);
    var hostName = location.hostname;
    var url = location.href;
    
    // Above the location object is used on every line
    
    with(location){
        var qs = search.substring(1);
        var hostName = hostname;
        var url = href;
    }
```

**In strict mode, the `with` statement is not allowed and is considered a syntax error.**

## The `switch` Statement
Syntax:
```javascript
    switch (expression) {
      case value: statement
        break;
      case value: statement
        break;
      default: statement
    }
```
It's best to always put a `break` statement after each case to aviod having cases fall through into the next one.

* Especially, in ECMAScript, `switch` statement works with all data types
* The case values need not to be constants; they can be variables and even expressions.

```javascript
    switch ("hello world") {
        case "hello" + " world":
            alert("Greeting was found.");
            break;
        case "goodbye":
            alert("Closing was found."):
            break;
        default:
            alert("Unexpected message was found.");
    }
    
    // another example
    var num = 25;
    switch (true) {
        case num < 0:
            alert("Less than 0.");
            break;
        case num >= 0 && num <= 10:
            alert("Between 0 and 10.");
            break;
        default:
            alert("More than 20.");
    }
```

**The `switch` statement compares values using the identically equal operator, so no type coercion occurs.**
