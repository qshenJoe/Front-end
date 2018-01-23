# Understanding Objects
The simplest way to create a custom object is to create a new instance of `Object` and add properties and methods to it.

```javascript
    var person = new Object();
    person.name = "Joe";
    person.age = 21;
    person.job = "Web Developer";
    person.sayName = function() {
        alert(this.name);
    };
    
    // object literals became the preferred pattern for creating such objects
    
    var person = {
        name: "Joe",
        age: 21,
        job: "Web Developer"
        sayName: function() {
            alert(this.name);
        }
    };
```

## Types of Properties
* An attribute is internal, surround the attribute name with two pairs of square brackets, such as `[[Enumerable]]`
<br />
There are two types of properties: data properties and accessor properties

### Data Properties
Data properties contain a single location for a data value. Values are read from and written to this location. Data properties have four attributes describing their behavior:
* `[[Configurable]]` - indicates if the property may be redefined by removing the property via `delete`, changing the property's attributes, or changing the property into an accessor property. By default, this is true for all properties defined directly on an object.
* `[[Enumerable]]` - indicates if the property will be returned in a `for-in` loop. By default, this is true for all properties defined directly on an object.
* `[[Writable]]` - indicates if the property's value can be changed. By default, this is true for all properties defined directly on an object.
* `[[Value]]` - Contains the actual data value for the property. This is the location from which the property's value is read and the location to which new values are saved. The default value for this attribute is `undefined`.
<br />
When a property is explicitly added to an object, `[[Configurable]]`, `[[Enumerable]]`, and `[[Writable]]` are all set to true while the `[[Value]]` attribute is set to the assigned value.

```javascript
    var person = {
        name: "Joe"
    };
    
    /*
     * The property called name is created and 
     * a value of "Joe" is assigned.
     * This means [[Value]] is set to "Joe",
     * and any changes to that value are
     * stored in this location
     */
```

To change any of the default property attributes, you must use the ECMAScript 5 `Object.defineProperty()` method. This method accepts three arguments:
* the object on which property should be added or modified
* the name of the property
* and a descriptor object - the properties on the descriptor object match the attribute names:
  * `configurable`
  * `enumerable`
  * `writable`
  * `value`
  
```javascript
    var person = {};
    Object.defineProperty(person, "name", {
        writable: false,    // define the property as read-only
        value: "Joe"
    });
    
    alert(person.name);   // "Joe"
    person.name = "Qiaohong";
    alert(person.name);   // "Joe"
    
    /*
     * Any attempts to assign a new value are 
     * ignored in nonstrict mode.
     * In strict mode, an error is thrown when an 
     * attempt is made to change the value of a 
     * read-only property.
     */
     
     // Similar rules apply to creating a nonconfigurable property
     var person = {};
     Object.defineProperty(person, "name", {
         configurable: false,
         value: "Joe"
     });
     
     alert(person.name);    // "Joe"
     delete person.name;
     alert(person.name);    // "Joe"
     
     /*
      * Calling delete on the property has no effect
      * in nonstrict mode and throws an error in strict
      * mode.
      * Additionally, once a property has been defined as 
      * nonconfigurable, it cannot become configurable again.
      * Any attempt to call Object.defineProperty() and change
      * any attribute other than writable causes an error.
      */
      
      var person = {};
      Object.defineProperty(person, "name", {
          configurable: false,
          value: "Joe"
      });
      
      // throws an error
      Object.defineProperty(person, "name", {
          configurable: true,
          value: "Joe"
      });
```

When you are using `Object.defineProperty()`, the values for `configurable`, `enumerable`, and `writable` default to false unless otherwise specified. In most cases, you likely won't need the powerful options provided by this method.

### Accessor Properties
Accessor properties do not contain a data value. Instead, they contain a conbination of a getter function and a setter function(though both are not necessary). When an accessor property is read from, the getter function is called, and it's the function's responsibility to return a valid value; When an accessor property is written to, a function is called with the new value, and that function must decide how to react to the data. Accessor properties have four aattributes:
* `[[Configurable]]` - indicates if the property may be redefined by removing the property via `delete`, changing the property's attributes, or changing the property into a data property. By default, this is true for all properties defined directly on an object.
* `[[Enumerable]]` - indicates if the property will be returned in a `for-in` loop. By default, this is true for all properties defined directly on an object.
* `[[Get]]` - the function to call when the property is read from. The default value is `undefined`.
* `[[Set]]` - the function to call when the property is written to. The default value is `undefined`.

```javascript
    /*
     * It is not possible to define an accessor property explicitly;
     * you must use Object.defineProperty()
     */
     
     var book = {
         _year: 2004,
         edition: 1
     };
     
     Object.defineProperty(book, "year", {
         get: function() {
             return this._year;
         }
         set: function() {
             if (newValue > 2004) {
                 this._year = newValue;
                 this.edition += newValue - 2004;
             }
         }
     });
     
     book.year = 2005;
     alert(book.edition);   // 2
     
     /* 
      * The underscore on _year is a common notation to indicate
      * that a property is not intended to be accessed from outside
      * of the object's methods.
      * The year property is defined to be an accessor property to 
      * determine the correct edition.
      * It's not necessary to assign both a getter and a setter.
      * Assigning just a getter means that the property cannot be written
      * to and attempts to do so will be ignored.
      * In strict mode, trying to write to a property with only a getter 
      * throws an error.
      * Likewise, a property with only a setter cannot be read and will return the value
      * undefined in nonstrict mode, while doing so throws an error in strict mode.
      */
      
      /*
       * Prior to the ECMAScript 5 method, two nonstandard methods 
       * were used to create accessor properties:
       * __defineGetter__() and __defineSetter__()
       */
       
       var book = {
           _year: 2004,
           edition: 1
       };
       
       // legacy accessor support
       book.__defineGetter__("year", function() {
           return this._year;
       });
       
       book.__defineSetter__("year", function() {
           if (newValue > 2004) {
               this._year = newValue;
               this.edition += newValue - 2004;
           }
       });
       
       book.year = 2005;
       alert(book.edition);   // 2
```

## Defining Multiple Properties
ECMAScript 5 provides the `Object.defineProperties()` method. There are two arguments:
* the object on which to add or modify the properties
* the object whose property names correspond to the properties's names to add or modify

```javascript
    var book = {};
    
    Object.defineProperties(book, {
        _year: {
            value: 2004
        },
        
        edition: {
            value: 1
        },
        
        year: {
            get: function() {
                return this._year;
            },
            
            set: function(newValue) {
                if (newValue > 2004) {
                    this._year = newValue;
                    this.edition += newValue - 2004;
                }
            }
        }
    });
    
    /* 
     * This code defines two data properties:
     * _year and edition, and an accessor property
     * called year on the book object.
     * This method allows creating all of these properties
     * at the same time.
     */
```

## Reading Property Attributes
It's possible to retrieve the property descriptor for a given property by using the ECMAScript 5 `Object.getOwnPropertyDescriptor()` method. This method accepts two arguments:
* the object on which the property resides
* the name of the property whose descriptor should be retrieved
<br />
The return value is an object with properties for `configurable`, `enumerable`, `get`, and `set` for accessor properties or `configurable`, `enumerable`, `writable`, and `value` for data properties.

```javascript
    var book = {};
    Object.defineProperties(book, {
        _year: {
            value: 2004
        },
        
        edition: {
            value: 1
        },
        
        year: {
            get: function() {
                return this._year;
            },
            
            set: function(newValue) {
                if (newValue > 2004) {
                    this._year = newValue;
                    this.edition += newValue - 2004;
                }
            }
        }
    });
    
    var descriptor = Object.getOwnPropertyDescriptor(book, "_year");
    alert(descriptor.value);    // 2004
    alert(descriptor.configurable);   // false
    alert(typeof descriptor.get);   // "undefined"
    
    var descriptor = Object.getOwnPropertyDescriptor(book, "year");
    alert(descriptor.value);    // undefined
    alert(descriptor.enumerable);   // false
    alert(typeof descriptor.get);   // "function"
```
