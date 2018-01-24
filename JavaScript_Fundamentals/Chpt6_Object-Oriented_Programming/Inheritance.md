# Inheritance
Many OO languages support two types of inheritance:
* interface inheritance - only method signatures are inherited
* implementation inheritance - actual methods are inherited
<br />
**However, interface inheritance is not possible in ECMAScript, because functions do not have signatures.**
<br />
Implementation inheritance is the only type of inheritance supported by ECMAScript, and this is done primarily through the use of prototype chaining.

## Prototype Chaining
ECMA-262 describes prototype chaining as the primary method of inheritance in ECMAScript. The basic idea is to use the concept of prototype to inherit properties and methods between two reference types.

```javascript
    /*
     * Implementing prototype chaining involves the following code pattern:
     */
     
    function SuperType() {
        this.property = true;
    }
    
    SuperType.prototype.getSuperValue = function() {
        return this.property;
    }
    
    function SubType() {
        this.subproperty = false;
    }
    
    // inherit from SuperType
    SubType.prototype = new SuperType();
    
    SubType.prototype.getSubValue = function() {
        return this.subproperty;
    };
    
    var instance = new SubType();
    alert(instance.getSuperValue());    // true
    
    /*
     * Note that instance.constructor points to SuperType,
     * because the constructor property on the SubType.prototype
     * was overwritten.
     */
```

Prototype chaining extends to the prototype search mechanism.
* When a property is accessed in read mode on an instance, the property is first searched for on the instance.
* If the property is not found, then the search continues to the prototype.
* When inheritance has been implemented via prototype chaining, that search can continue up the prototype chain.
* A call to `instance.getSuperValue()` results in a three-step search:
  * the instance
  * `SubType.prototype`
  * `SuperType.prototype`
* The search for properties and methods always continues until the end of the prototype chain is reached.

**Recall that \[[prototype\]] is an internal pointer to the constructor's prototype.**

## Constructor Stealing

## Combination Inheritance

## Prototypal Inheritance

## Parasitic Inheritance

## Parasitic Combination Inheritance
