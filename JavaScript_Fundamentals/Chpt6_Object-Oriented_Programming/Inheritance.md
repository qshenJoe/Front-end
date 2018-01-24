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
In an attempt to solve the inheritance problem with reference values on prototypes, developers began using a technique called *constructor stealing*(also sometimes called object masquerading or classical inheritance). 
* The basic idea is to call the supertype constructor from within the subtype constructor.
* Functions are simply objects that execute code in a particular context, the `apply()` and `call()` methods can be used to execute a constructor on the newly created object.

```javascript
    function SuperType() {
        this.colors = ["red", "blue", "green"];
    }
    
    function SubType() {
        // inherit from SuperType
        SuperType.call(this);
    }
    
    var instance1 = new SubType();
    instance1.colors.push("black");
    alert(instance1.colors);        // "red,blue,green,black"
    
    var instance2 = new SubType();
    alert(instance2.colors);        // "red,blue,green"
    
    /*
     * By using the call() method(or apply()), the SuperType constructor
     * is called in the context of the newly created instance of SubType.
     * Doing this effectively runs all of the object-iniltialization code 
     * in the SuperType() function on the new SubType object.
     * The result is that each instance has its own copy of the colors property.
     */
```

### Passing Arguments
One advantage that constructor stealing offers over prototype chaining is the ability to pass arguments into the supertype constructor from within the subtype constructor.

```javascript
    function SuperType(name) {
        this.name = name;
    }
    
    function SubType() {
        // inherit from SuperType passing in an argument
        SuperType.call(this, "Joe");
        
        // instance property
        this.age = 21;
    }
    
    var instance = new SubType();
    alert(instance.name);   // "Joe"
    alert(instance.age);    // 21
    
    /*
     * To ensure that the SuperType constructor does not overwrite those 
     * properties, you can define additional properties on the subtype
     * after the call to the supertype constructor.
     */
```

### Problems with Constructor Stealing
* it introduces the same problems as the constructor pattern from custom types
    * methods must be defined inside the constructor, so there's no function reuse.
    * methods defined on the supertype's prototype are not accessible on the subtype, so all types can use only the constructor pattern.

## Combination Inheritance
Sometimes also called pseudoclassical inheritance, which combines prototype chaining and constructor stealing to get the best of each approach.
* The basic idea is to use prototype chaining to inherit properties and methods on the prototype and to use constructor stealing to inherit instance properties.
* This allows function reuse by defining methods on the prototype and allows each instance to have its own properties.

```javascript
    function SuperType(name) {
        this.name = name;
        this.colors = ["red", "blue", "green"];
    }
    
    SuperType.prototype.sayName = function() {
        alert(this.name);
    };
    
    function SubType(name, age) {
        
        // inherit properties
        SuperType.call(this, name);
        
        this.age = age;
    }
    
    // inherit methods
    SubType.prototype = new SuperType();
    
    SubType.prototype = new SuperType();
    
    SubType.prototype.sayAge = function() {
        alert(this.age);
    };
    
    var instance1 = new SubType("Joe", 21);
    instance1.colors.push("black");
    alert(instance1.colors);    // "red,blue,green,black"
    instance1.sayName();    // "Joe"
    instance1.sayAge();     // 21
    
    var instance2 = new SubType("Qiaohong", 23);
    alert(instance2.colors);    // "red,blue,green"
    instance2.sayName();    // "Qiaohong"
    instance2.sayAge();     // 23
    
    /* 
     * The SuperType constructor defines two properties, name and colors,
     * and the SuperType prototype has a single method called sayName().
     * The SubType constructor calls the SuperType constructor, passing
     * in the name argument, and defines its own property called age.
     * Additionally, the SubType prototype is assigned to be an instance
     * of SuperType, and then a new method called sayAge() is defined. 
     * With this code, it's then possible to create two seperate instanes
     * of SubType that have their own properties, including the colors
     * property, but all use the same methods.
     * It also preserves the behavior of instanceof and isPrototypeOf()
     * for identifying the composition of objects.
     */
```

## Prototypal Inheritance
Create new objects based on existing objects without the need for defining custom types.

```javascript
    function object(o) {
        function F() {}
        F.prototype = o;
        return new F();
    }
    
    /*
     * The object() function creates a temporary constructor, assigns a given
     * object as the constructor's prototype, and returns a new instance of the
     * temporary type. 
     * Essentially, object() performs a shadow copy of any object that it
     * passed into it.
     */
     
     var person = {
         name: "Joe",
         friends: ["Mark", "Charles", "Terry"]
     };
     
     var anotherPerson = object(person);
     anotherPerson.name = "Qiaohong";
     anotherPerson.friends.push("Tony");
     
     var yetAnotherPerson = object(person);
     yetAnotherPerson.name = "Shen";
     yetAnotherPerson.friends.push("Cyntia");
     
     alert(person.friends);     // "Mark,Charles,Terry,Tony,Cyntia"
     
     /*
      * You have an object that you want to use as the base of another object.
      * That object should be passed into object(), and the resulting object
      * should be modified accordingly.
      * The person object contains information that should be available on 
      * another object, so it is passed into the object() function, which
      * returns a new object.
      * The new object has person as its prototype, meaning that it has both
      * a primitive value property and a reference value property on its 
      * prototype. This also means that person.friends is shared not only
      * by person but also with anotherPerson and yetAnotherPerson.
      * This effectively create two clones of person.
      */
```

ECMAScript 5 formalized the concept of prototypal inheritance by adding the `Object.create()` method.
* This method accepts two arguments
    * an object to use as the prototype for a new object
    * an optional object defining additional properties to apply to the new object
* When used with one argument, `Object.create()` behaves the same as the `object()` method:

```javascript
    var person = {
        name: "Joe",
        friends: ["Mark", "Charles", "Terry"]
    };
    
    var anotherPerson = Object.create(person);
    anotherPerson.name = "Qiaohong";
    anotherPerson.friends.push("Tony");
    
    var yetAnotherPerson = Object.create(person);
    yetAnotherPerson.name = "Shen";
    yetAnotherPerson.friends.push("Cyntia");
    
    alert(person.friends);      // "Mark,Charles,Terry,Tony,Cyntia"
    
    /* 
     * The second argument for Object.create() is in the same format as the second
     * argument for Object.defineProperties(): each additional property to define
     * is specified along with its descriptor.
     * Any properties specified in this manner will shadow properties of the same
     * name on the prototype object.
     */
     
     var person = {
         name: "Joe",
         friends: ["Mark", "Charles", "Terry"]
     };
     
     var anotherPerson = Object.create(person, {
         name: {
             value: "Qiaohong"
         }
     });
     
     alert(anotherPerson.name);     // "Qiaohong"
```

Prototypal inheritance is useful when there is no need for the overhead of creating separate constructors, but you still need an object to behave similarly to another. <br />
**Properties containing reference values will always share those values, similar to using the prototype pattern.**

## Parasitic Inheritance
Closely related to prototypal inheritance is the concept of parasitic inheritance. The idea behind parasitic inheritance is similar to that of the parasitic constructor and factory patterns:
* create a function that does the inheritance, augments the object in some way, and returns the object as if it did all the work.

```javascript
    function createAnother(original) {
        var clone = object(original);   // create a new object by calling a function
        clone.sayHi = function() {      // augment the object in some way
            alert("hi");
        };
        return clone;       // return the object
    }
    
    /* 
     * The createAnother() function accepts a single argument, which is the object to 
     * base a new object on. This object, original, is passed into the object() function,
     * and the result is assigned to clone.
     * Next, the clone object is changed to have a new method called sayHi().
     * The last step is to return the object.
     */
     
     /*
      * The createAnother() can be used in the following way
      */
      
      var person = {
          name: "Joe",
          friends: ["Mark", "Charles", "Terry"]
      };
      
      var anotherPerson = createAnother(person);
      anotherPerson.sayHi();        // "hi"
      
      /*
       * The code in this example returns a new object based on person.
       * The anotherPerson object has all of the properties and methods
       * of person but adds a new method called sayHi().
       *
       * Parasitic inheritance is another pattern to use when you are
       * concerned primarily with objects and not with custom types and 
       * constructors.
       * The object() method is not required for parasitic inheritance;
       * any function that returns a new object fits the pattern.
       */
```

## Parasitic Combination Inheritance
Combination inheritance is the most often-used pattern for inheritance in JavaScript, though it is not without its inefficiencies. The most inefficient part if the pattern is that the supertype constructor is always called twice: once to create the subtype's prototype, and once inside the subtype constructor. Essentially, the subtype prototype ends up with all of the instance properties of a supertype object, only to have it overwritten when the subtype constructor executes.

```javascript
    function SuperType(name) {
        this.name = name;
        this.colors = ["red", "blue", "green"];
    }
    
    SuperType.prototype.sayName = function() {
        alert(this.name);
    };
    
    function SubType(name, age) {
        SuperType.call(this, name);     // second call to SuperType()
        
        this.age = age;
    }
    
    SubType.prototype = new SuperType();    // first call to SuperType()
    SubType.prototype.constructor = SubType;
    SubType.prototype.sayAge = function() {
        alert(this.age);
    };
    
    /*
     * There will be two sets of name and colors properties:
     * one on the instance and one on the SubType prototype.
     * This is the result of calling the SubType constructor
     * twice.
     */
```

Parasitic combination inheritance uses constructor stealing to inherit properties but uses a hybrid form of prototype chaining to inherit methods. The basic idea is:
* Instead of calling the supertype constructor to assign the subtype's prototype, all you need is a copy of the supertype's prototype. Essentially, use parasitic inheritance to inherit from the supertype's prototype and then assign the result to the subtype's prototype.

```javascript
    function inheritPrototype(subType, superType) {
        var prototype = object(superType.prototype);    // create object
        prototype.constructor = subType;        // augment object
        subType.prototype = prototype;      // assign object
    }
    
    /* 
     * The inheritPrototype() function implements very basic parasitic combination inheritance.
     * This function accepts two arguments: the subtype constructor and the supertype constructor.
     * Inside the function, the first step is to create a clone of the supertype's prototype.
     * Next, the constructor property is assigned onto prototype to account for losing the default
     * constructor property when the prototype is overwritten.
     * Finally, the subtype's prototype is assigned to the newly created object.
     * A call to inheritPrototype() can replace the subtype prototype assignment in the previous example
     */
     
     function SuperType(name) {
         this.name = name;
         this.colors = ["red", "blue", "green"];
     }
     
     SuperType.prototype.sayName = function() {
         alert(this.name);
     };
     
     function SubType(name, age) {
         SuperType.call(this, name);
         
         this.age = age;
     }
     
     inheritPrototype(SubType, SuperType);
     
     SubType.prototype.sayAge = function() {
         alert(this.age);
     };
     
     /*
      * This example is more efficient in that the SuperType constructor is being called only one
      * time, avoiding having unnecessary and unused properties on SubType.prototype.
      * Furthermore, the prototype chain is kept intact, so both instanceof and isPrototypeOf()
      * behave as they would normally.
      * Parasitic combination inheritance is considered the most optimal inheritance paradigm for 
      * reference types.
      */
```
