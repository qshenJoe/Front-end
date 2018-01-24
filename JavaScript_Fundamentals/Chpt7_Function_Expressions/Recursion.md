# Recursion
A recursive function typically is formed when a function calls itself by name.

```javascript
    function fctorial(num) {
        if (num <= 1) {
            return 1;
        } else {
            return num * factorial(num - 1);
        }
    }
    
    /*
     * Although this works initially, it's possible to
     * prevent it from functioning by running the 
     * following code immediately after it
     */
     
    var anotherFactorial = factorial;
    factorial = null;
    alert(anotherFactorial(4));   // error
    
    /* 
     * Here the factorial() function is store in a variable called anotherFactorial.
     * Then the factorial variable is set to null, so only one reference to the 
     * original function remains. When anotherFactorial() is called, it will try 
     * to execute factorial(), which is no longer a function. Using arguments.callee
     * can alleviate this problem.
     */
     
    /*
     * arguments.callee is a pointer to the function being executed and, 
     * as such, can be used to call the function recursively.
     */
    
    function factorial(num) {
        if (num <= 1) {
            return 1;
        } else {
            return num * arguments.callee(num - 1);
        }
    }
```

The value of `arguments.callee` is not accessible to a script running in strict mode and will cause an error when attempts are made to read it. Instead, you can use named function expressions to achieve the same result.

```javascript
    var factorial = (function f(num) {
        if (num <= 1) {
            return 1;
        } else {
            return num * f(num - 1);
        }
    });
    
    /*
     * A named function expression f() is created and assigned to the variable factorial.
     * The name f remains the same even if the function is assigned to another variable,
     * so the recursive call will always execute correctly.
     * This pattern works in both nonstrict mode and strict mode.
     */
```



