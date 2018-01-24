# Closures
Closures are functions that have access to variables from another function's scope. This is often accomplished by creating a function inside a function.

```javascript
    function createComparisonFunction(propertyName) {
        return function(object1, object2) {
            var value1 = object1[propertyName];
            var value2 = object2[propertyNmae];
            
            if (value1 < value2) {
                return -1;
            } else if (value1 > value2) {
                return 1;
            } else {
                return 0;
            }
        };
    }
    
    /*
     * Consider the following
     */
    
    function compare(value1, value2) {
        if (value1 < value2) {
            return -1;
        } else if (value1 > value2) {
            return 1;
        } else {
            return 0;
        }
    }
    
    var result = compare(5, 10);
    
    /*
     * The code defines a function named compare() that is called in the global execution context.
     * When compare() is called for the first time, a new activation object is created that contains
     * arguments, value1, and value2. The global execution context's variable object is next in the 
     * compare() execution context's scope chain, which contains this, result, and compare.
```
## Closures and Variables

## The this Object

## Memory Leaks
