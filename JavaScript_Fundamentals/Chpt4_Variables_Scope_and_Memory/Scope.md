# Execution Context and Scope
* Global execution context in web browser: `window` object
* A scope chain is to provide ordered access to all variables and functions that an execution context has access to.
  * The front of the scope chain is alwanys the variable object of the context whose code is executing.
  * The global context's variable object is always the last of the scope chain.
  
```javascript
    var color = "red";
      
    function changeColor() {
        if (color === "red") {
            color = "blue";
        } else {
           color = "red";
        }
    }
      
    changeColor();  // The variable color is accessible inside the function, because it can be found in the scope chain
      
    /*
     * this function changeColor() has a scope chain with two objects in it:
     * its own variable object(upon which the arguments object is defined)
     * and the global context's variable object
     */
```
  
In addition, locally defined variables can be used interchangeably with global variables in a local context:
  
```javascript
    var str1 = "aaa";
    
    function changeString() {
        var str2 = "bbb";
          
        function swapStrings() {
            var tempStr = str2;
            st2 = str1;
            str1 = tempStr;
            
            // str1, str2, and the tempStr are all accessible here
      }
          
            // str1 and str2 are accessible here, but not tempStr
          
          swapStrings();
      }
      
      //only str1 is accessible here
      changeString();
```
  
Scope chain for the above example: <br />
<pre>
window
  |_str1
  |_changeString()
    |_str2
    |_swapStrings()
      |_tempStr
</pre>

**Function arguments are considered to be variables and follow the same access rule as any other variable in the execution context.**

## Scope Chain Augmentation
Ways to augment the scope chain:
* The `catch` block in a `try-catch` statement
* A `with` statement

Both of these statements add a variable object to the front of the scope chain.

```javascript
    function buildUrl() {
        var qs = "?debug=true";
        
        with(location) {    // location object is added to the front of the scope chain
            var url = href + qs;
        }
        
        return url;
    }
```

## No Block-Level Scopes
An example:

```javascript
    if (true) {
        var color = "blue";
    }
    
    alert(color);    // "blue"
    
    /*
     * In JavaScript, the variable declaration
     * adds a variable into the current execuction
     * context(the global context in this case)
     */
```

Another example:

```javascript
    for (var i = 0; i < 10; i++) {
        doSomething(i);
    }
    
    alert(i);    // 10
    
    /*
     * In JavaScript, the i varibale is created
     * by the for statement and continues to exist
     * outside the loop after the statement executes.
     */
```

### Variable Declaration
When a variable is declared using `var`, it is autmatically added to the most immediate context available.
* In a function, the most immediate one is the function's local context.
* In a with statement, the most immediate is the function context.
* If a variable is initialized without first being declared, it gets added to the global context automatically.

```javascript
    function add(num1, num2) {
        var sum = num1 + num2;
        return sum;
    }
    
    var result = add(10, 20);    // 30
    alert(sum);    // causes an error because sum is not a valid variable
    
    function add(num1, num2) {
        sum = num1 + num2;    // declared without var
        return sum;
    }
    
    var result = add(10, 20);    // 30
    alert(sum);    // 30
```

### Indentifier Lookup
Objects in the scope chain also have a prototype chain, so searching may include each object's prototype chain.

```javascript
    var color = "blue";
    
    function getColor() {
        return color;    // identifier lookup occurs, not finding color inside the function context
    }
    
    // identifier lookup continues to search the global context
    
    alert(getColor());    // "blue"
```

<pre>
window   2 <-
  |_color    | 
  |_getColor  - 1 <-
                    |
                    |
    return color   - 
</pre>

Another example:

```javascript
    var color = "blue";
    
    function getColor() {
        var color = "red";
        return color;
    }
    
    alert(getColor());    // red
```

Any line of code appearing after the declaration of `color` as a local variable cannot access the global `color` variable without qualifying it as `window.color`.
