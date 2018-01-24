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

**Recall that \[[Prototype\]] is an internal pointer to the constructor's prototype.**

### Default Prototypes
All reference types inherit from `Object` by default, which is accomplished through prototype chaining.
* The default prototype for any function is an instance of `Object`, meaning that its internal prototype pointer points to `Object.prototype`. 
* This is how custom types inherit all of the default methods such as `toString()` and `valueOf()`.

### Prototype and Instance Relationship
* use the `instanceof` operator, which returns `true` whenever an instance is used with a constructor that appears in its prototype chain

```javascript
    alert(instance instanceof Object);      // true
    alert(instance instanceof SuperType);   // true
    alert(instance instanceof SubType);     // true
```

* use `isPrototypeOf()` method. Each prototype in the chain has access to this method, which returns `true` for an instance in the chain.

```javascript
    alert(Object.prototype.isPrototypeOf(instance));        // true
    alert(SuperType.prototype.isPrototypeOf(instance));     // true
    alert(SubType.prototype.isPrototypeOf(instance));       // true
```

### Working with Methods
Often a subtype will need to either override a supertype method or introduce new methods that don't exist on the supertype. To accomplish this, the methods must be added to the prototype after the prototype has been assigned.

```javascript
    function SuperType() {
        this.property = true;
    }
    
    SuperType.prototype.getSuperValue = function() {
        return this.property;
    };
    
    function SubType() {
        this.subproperty = false;
    }
    
    // inherit from SuperType
    SubType.prototype = new SuperType();
    
    // new method
    SubType.prototype.getSubValue = function() {
        return this.subproperty;
    };
    
    // override existing method
    SubType.prototype.getSuperValue = function() {
        return false;
    }
    
    var instance = new SubType();
    alert(intance.getSuperValue());     // false
    
    /*
     * Both of the methods are defined after the prototype
     * has been assigned as an instance of SuperType
     */
     
     /*
      * The object literal approach to creating prototype
      * methods cannot be used with prototype chaining,
      * because you end up overwriting the chain.
      */
      
      function SuperType() {
          this.property = true;
      }
      
      SuperType.prototype.getSuperValue = function() {
          return this.property;
      };
      
      function SubType() {
          this.subproperty = false;
      }
      
      // inherit from SuperType
      SubType.prototype = new SuperType();
      
      // try too add new methods - this nullifies the previous line
      SubType.prototype = {
          getSubValue: function() {
              return this.subproperty;
          },
          
          someOtherMethod: function() {
              return false;
          }
      };
      
      var instance = new SubType();
      alert(instance.getSuperValue());      // error
      
      /*
       * The prototype now contains a new instance of Object
       * instead of an instance of SuperType, so the prototype
       * chain has been broken - there is no relationship
       * between SubType and SuperType.
       */
```

### Problems with Prototype Chaining
The major issue revolves around prototypes that contain reference values.
* Prototype properties containing reference values are shared with all instances; this is why properties are typically defined within the constructor instead of on the prototype.
* When implementing inheritance using prototypes, the prototype actually becomes an instance of another type, meaning that what once were instance properties are now prototype properties.

```javascript
    function SuperType() {
        this.colors = ["red", "blue", "green"];
    }
    
    function SubType() {
    }
    
    // inherit from SuperType
    SubType.prototype = new SuperType();
    
    var instance1 = new SubType();
    instance1.colors.push("black");
    alert(instance1.colors);    // "red,blue,green,black"
    
    var instance2 = new SubType();
    alert(instance2.colors);    // "red,blue,green,black"
    
    /*
     * When SubType inherits from SuperType via prototype chaining, SubType.prototype
     * becomes an instance of SuperType and so it gets its own colors property, which
     * is akin to specifically creating SubType.prototype.colors.
     * The end result is that all instances of SubType share a colors property.
     */
```

A second issue with prototype chaining is that you cannot pass arguments into the supertype constructor when the subtype instance is being created. In fact, there is no way to pass arguments into the supertype constructor without affecting all of the object instances. <br />
So, prototype chaining is rarely used alone.

## Constructor Stealing

## Combination Inheritance

## Prototypal Inheritance

## Parasitic Inheritance

## Parasitic Combination Inheritance
