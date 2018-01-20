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

## Scope Chain Augmentation

## No Block-Level Scopes
