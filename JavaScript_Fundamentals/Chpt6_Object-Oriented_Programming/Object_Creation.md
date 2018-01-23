# Object Creation
Downside of using `Object` constructor or an object literal: creating multiple objects with the same interface requires a lot of code duplication.

## The Factory Pattern
The factory pattern is a well-known design pattern used in software engineering to abstract away the process of creating specific objects. With no way to define classes in ECMAScript, developers created functions to encapsulate the creation of objects with specific interfaces:

```javascript
    function createPerson(name, age, job) {
        var o = new Object();
        o.name = name;
        o.age = age;
        o.job = job;
        o.sayName = function() {
            alert(this.name);
        };
        return o;
    }
    
    var person1 = createPerson("Joe", 21, "Web Developer");
    var person2 = createPerson("Qiaohong", 23, "Gamer");
    
    /*
     * Though this solved the problem of creating multiple similar objects,
     * the factory pattern didn't address the issue of object identification
     * (what type of object an object is). As JavaScript continued to evolve,
     * a new pattern emerged.
     */
```

## The Constructor Pattern
It's possible to define custom constructors that define properties and methods for your own type of object.

```javascript
    function Person(name, age, job) {
        this.name = name;
        this.age = age;
        this.job = job;
        this.sayName = function() {
            alert(this.name);
        };
    }
    
    var person1 = new Person("Joe", 21, "Web Developer");
    var person2 = new Person("Qiaohong", 23, "Gamer");
    
    /*
     * The Person() function takes the place of factory createPerson() function.
     */
```

Note that the code inside `Person()` is the same as the code inside `createPerson()`, with the following exceptions:
* There is no object being created explicitly
* The properties and method are assigned directly onto the `this` object
* There is no `return` statement

Note the name of the function is `Person` with an uppercase *P*. By convention, constructor functions always begin with an uppercase letter, whereas nonconstructor functions begin with a lowercase letter.
<br />
<br />
To create a new instance of `Person`, use the `new` operator. Calling a constructor in this manner essentially causes the following four steps to be taken:
* create a new object
* assign the `this` value of the constructor to the new object(so `this` points to the new object)
* execute the code inside the constructor(add properties to the new object)
* return the new object

```javascript

    /*
     * Each of these objects has a constructor property that points back to Person
     */
     
    alert(person1.constructor == Person);   // true
    alert(person2.constructor == Person);   // true
     
    
    /*
     * The constructor property was originally intended for use in
     * identifying the object type. However, the instanceof operator
     * is considered to be a safer way of determining type.
     */
     
     alert(person1 instanceof Object);    // true
     alert(person1 instanceof Person);    // true
     alert(person2 instanceof Object);    // true
     alert(person2 instanceof Person);    // true
     
     
     // All custom objects inherit from Object.
```

### Constructors as Functions
Constructors are just functions. Any function that is called with the `new` operator acts as a constructor and vice versa.
For instance, the `Person()` function from the previous example may be called in any of the following ways:

```javascript
    // use as a constructor
    var person = new Person("Joe", 21, "Web Developer");
    person.sayName();   // "Joe"
    
    // call as a function
    Person("Joe", 21, "Web Developer");   // adds to window because this points to window now
    window.sayName();   // "Joe"
    
    // call in the scope of another object
    var o = new Object();
    Person.call(o, "Joe", 21, "Web Developer");
    o.sayName();    // "Joe"
    
    /*
     * The Person() function can also be called within the scope
     * of a particular object using call()(or apply()).
```

### Problems with Constructors
The major downside to constructors is that methods are created once for each instance. So, for `person1` and `person2`, both instances have a menthod called `sayName()`, but those methods are not the same instance of `Function`. Remember, functions are objects in ECMAScript, so every time a function is defined, it's actually an object being instantiated.

```javascript
    function Person(name, age, job) {
        this.name = name;
        this.age = age;
        this.job = job;
        this.sayName = new Function("alert(this.name)");        // logical equivalent
    }
    
    /*
     * it's clear that each instance of Person gets
     * its own instance of Function that happen to
     * display the name property.
     * Creating a function in this manner is different with regard to
     * scope chains and identifier resolution, but the mechanics of 
     * creating a new instance of Function remain the same.
     * So, functions of the same name on different instances are not
     * equivalent.
     */
     
     alert(person1.sayName == person2.sayName);     // false
     
     /*
      * It doesn't make sense to have two instances of Function
      * that do the same thing, especially when the this object
      * makes it possible to avoid binding functions to particular
      * objects until runtime.
      * It's possible to work around this limitation by moving
      * the function definition outside of the constructor.
      */
      
      function Person(name, age, job) {
          this.name = name;
          this.age = age;
          this.job = job;
          this.sayName = sayName;
      }
      
      function sayName() {
          alert(this.name);
      }
      
      var person1 = new Person("Joe", 21, "Web Developer");
      var person2 = new Person("Qiaohong", 23, "Gamer");
      
      /*
       * If the object needed multiple methods, that would realistically be used only
       * in relation to an object. If the object needed multiple methods, that would
       * mean multiple global functions, and all of a sudden the custom reference type
       * definition is no longer nicely grouped in the code.
       */
```

## The Prototype Pattern
Each function is created with a `prototype` property, which is an object containing properties and methods that should be available to instances of a particular reference type. This object is literally a prototype for the object to be created once the constructor is called.
* The benefit of using the prototype is that all of its properties and methods are shared among object instances.
* Instead of assigning object information in the constructor, they can be assigned directly to the prototype.

```javascript
    function Person() {
    }
    
    Person.prototype.name = "Joe";
    Person.prototype.age = 21;
    Person.prototype.job = "Web Developer";
    Person.prototype.sayName = function() {
        alert(this.name);
    };
    
    var person1 = new Person();
    person1.sayName();  // "Joe"
    
    var person2 = new Person();
    person2.sayName();  // "Joe"
    
    alert(person1.sayName == person2.sayName);      // true
```

### How Prototypes Work
Whenever a function is created, its `prototype` property is also created according to a specific set of rules.
* By default, all prototypes automatically get a property called `constructor` that points back to the function on which it is a property.
* For instance, `Person.prototype.constructor` points to `Person`
* Then, depending on the constructor, other properties and methods may be added to the prototype
<br />
When defining a custom constructor, the prototype gets the `constructor` property only by default; all other methods are inherited from `Object`.
* Each time the constructor is called to create a new instance, that instance has an internal pointer to the constructor's prototype. In ECMA-262 fifth edition, this is called `[[Prototype]]`
* The important thing to understand is that a direct link exists between the instance and the constructor's prototype but not between the instance and the constructor.

```javascript
    /*
     * Even though [[Prototype]] is not accessible in all implementation, the isPrototypeOf() method
     * can be used to determine if this relationship exists between objects.
     * Esstentially, isPrototypeOf() returns if [[Prototype]] points to the prototype on which
     * the method is being called
     */
     
     alert(Person.prototype.isPrototypeOf(person1));        // true
     alert(Person.prototype.isPrototypeOf(person2));        // true
     
     /*
      * ECMAScript 5 adds a new method called Object.getPrototypeOf(), 
      * which returns the value of [[Prototype]] in all supporting implementations.
      */
      
      alert(Object.getPrototypeOf(person1) == Person.prototype);    // true
      alert(Object.getPrototypeOf(person1).name);   // "Joe"
      
      /*
       * It's important to use Object.getPrototypeOf() if you want to
       * implement inheritance using the prototype
       */
```

Whenever a property is accessed for reading on an object, a search is started to find a property with that name. The search begins on the object instance itself.
* If a property with the given name is found on the instance, then that value is returned.
* If the property is not found, then the search continues up the pointer to the prototype, and the prototype is searched for a property with the same name. If the property is found on the prototype, then that value is returned.

```javascript
    /*
     * Although it's possible to read values on the prototype from object instances, it
     * is not possible to overwrite them.
     * If you add a property to an instance that has the same name as a prototype on
     * the prototype, you create the property on the instance, which then makes the property
     * on the prototype.
     */
     
     function Person() {
     }
     
     Person.prototype.name = "Joe";
     Person.prototype.age = 21;
     Person.prototype.job = "Web Developer";
     Person.prototype.sayName = function() {
        alert(this.name);
     };
     
     var person1 = new Person();
     var person2 = new Person();
     
     person1.name = "Qiaohong";
     alert(person1.name);   // "Qiaohong" - from instance
     alert(person2.name);   // "Joe" - from prototype
     
     /*
      * When we add a new property to an instance with the same name of the property
      * in the prototype, the new property shadows the one in the prototype, and blocks 
      * the access to the property in the prototype because the search only happens in the
      * instance. And as soon as it finds the property, the search is terminated.
      */
```

Even setting the property to `null` only sets the property on the instance and doesn't restore the link to the prototype. The `delete` operator, however, completely removes the instance property and allows the `prototype` property to be accessed again.

```javascript
    function Person() {
    }
    
    Person.prototype.name = "Joe";
    Person.prototype.age = 21;
    Person.prototype.job = "Web Developer";
    Person.prototype.sayName = function() {
        alert(this.name);
    };
    
    var person1 = new Person();
    var person2 = new Person();
    
    person1.name = "Qiaohong";
    alert(person1.name);    // "Qiaohong" - from instance
    alert(person2.name);    // "Joe" - from prototype
    
    delete person1.name;
    alert(person1.name);    // "Joe" - from the prototype
    
    /*
     * The delete operation restores the link to the prototype's 
     * name property, so the next time person1.name is accessed,
     * it's the prototype property's value that is returned.
     */
```

The `hasOwnProperty()` method determines if a property exists on the instance or on the prototype. This method, which is inherited from `Object`, returns `true` only if a property of the given name exists on the object instance:

```javascript
    function Person() {
    }
    
    Person.prototype.name = "Joe";
    Person.prototype.age = 21;
    Person.prototype.job = "Web Developer";
    Person.prototype.sayName = function() {
        alert(this.name);
    };
    
    var person1 = new Person();
    var person2 = new Person();
    
    alert(person1.hasOwnProperty("name"));  // false
    
    person1.name = "Qiaohong";
    alert(person1.name);        // "Qiaohong" - from instance
    alert(person1.hasOwnProperty("name"));  // true
    
    alert(person2.name);    // "Joe" from prototype
    alert(person2.hasOwnProperty("name"));  // false
    
    delete person1.name;
    alert(person1.name);    // "Joe" - from the prototype
    alert(person1.hasOwnProperty("name"));  // false
```

**The ECMAScirpt 5 `Object.getOwnPropertyDescriptor()` method works only on instance properties; to retrieve the descriptor of a prototype, you must call `Object.getOwnPropertyDescriptor()` on the prototype object directly.**

### Prototypes and the `in` Operator
There are two ways to use the `in` operator:
* On its own
    * When used on its own, the `in` operator returns `true` when a property of the given name is accessible by the object, which is to say that the property may exist on the instance or on the prototype.

```javascript
    function Person() {
    }
    
    Person.prototype.name = "Joe";
    Person.prototype.age = 21;
    Person.prototype.job = "Web Developer";
    Person.prototype.sayName = function() {
        alert(this.name);
    };
    
    var person1 = new Person();
    var person2 = new Person();
    
    alert(person1.hasOwnProperty("name"));  // false
    alert("name" in person1);   // true - exist in the prototype
    
    person1.name = "Qiaohong";
    alert(person1.name);    // "Qiaohong" - from instance
    alert(person1.hasOwnProperty("name"));  // true
    alert("name" in person1);   // true - exist in both instance and prototype
    
    alert(person2.name);    // "Joe" - from prototype
    alert(person2.hasOwnProperty());    // false
    alert("name" in person2);   // true - exist in the prototype
    
    delete person1.name;
    alert(person1.name);    // "Joe" - from prototype
    alert(person1.hasOwnProperty());    // false
    alert("name" in person1);   // true - exist in the prototype
    
    /*
     * it's possible to determine if the property of an object exists on the prototype
     * by combining a call to hasOwnProperty() with the in operator
     */
     
     function hasPrototypeProperty(object, name) {
         return !object.hasOwnProperty(name) && (name in object);
     }
     
     function Person() {
     }
     
     Person.prototype.name = "Joe";
     Person.prototype.age = 21;
     Person.prototype.job = "Web Developer";
     Person.prototype.sayName = function() {
         alert(this.name);
     };
     
     var person = new Person();
     alert(hasPrototypeProperty(person, "name"));   // true
     
     person.name = "Qiaohong";
     alert(hasPrototypeProperty(person, "name"));   // false
```

* as a `for-in` loop
    * all properties that are accessible by the object and can be enumerated will be turned, which includes properties both on the instance and on the prototype.
    * Instance properties that shadow a non-enumerable `prototype` property(a property that has `[[Enumerable]]` set to false) will be returned in the `for-in` loop, since all developer-defined properties are enumerable by rule.

```javascript
    /*
     * The old IE implementation has a bug where properties that shadow non-enumerable properties will not show up
     * in a for-in loop
     */
     
     var o = {
         toString : function() {
             return "My Object";
         }
     };
     
     for (var prop in o) {
         if (prop == "toString") {
             alert("Found toString");   // won't display in Internet Explorer
         }
     }
```

To retrieve a list of all enumerable instance properties on an object, you can use the ECMAScript 5 `Object.keys()` method, which accepts an object as its argument and returns an array of strings containing the names of all enumerable properties.

```javascript
    function Person() {
    }
    
    Person.prototype.name = "Joe";
    Person.prototype.age = 21;
    Person.prototype.job = "Web Developer";
    Person.prototype.sayName = function() {
        alert(this.name);
    };
    
    var keys = Object.keys(Person.prototype);
    alert(keys);    // "name,age,job,sayName" - prototype properties
    
    var p1 = new Person();
    p1.name = "Rob";
    p1.age = 31;
    var p1keys = Object.keys(p1);
    alert(p1keys);  // "name,age" - instance properties
    
    /*
     * If you'd like a list of all instance properties, whether enumerable or not, you can use 
     * Object.getOwnPropertyNames() in the same way:
     */
     
     var keys = Object.getOwnPropertyNames(Person.prototype);
     alert(keys);   // "conostructor,name,age,job,sayName"
     
     /*
      * Note the inclusion of the non-enumerable constructor property in the list
      * of results. Both Object.keys() and Object.getOwnPropertyNames() may be 
      * suitable replacements for using for-in.
      */
```

### Alternate Prototype Syntax
To limit this redundancy and to better visually encapsulate functionality on the prototype, it has become more common to simply overwrite the prototype with an object literal that contains all of the properties and methods.

```javascript
    function Person() {
    }
    
    Person.prototype = {
        name: "Joe",
        age: 21,
        job: "Web Developer",
        sayName: function() {
            alert(this.name);
        }
    };
    
    /*
     * The only difference is that the constructor property
     * no longer points to Person.
     * Although the instanceof operator still works reliably,
     * you cannot rely on the constructor to indicate the type
     * of object.
     */
     
     var friend = new Person();
     alert(friend instanceof Object);   // true
     alert(friend instanceof Person);   // true
     alert(frined.constructor == Person);   // false
     alert(friend.constructor == Object);   // true
     
     /*
      * If the constructor's value is important,
      * you can set it specifically back to the 
      * appropriate value.
      */
      
      function Person() {
      }
      
      Person.prototype = {
          constructor: Person,
          name: "Joe",
          age: 21,
          job: "Web Developer",
          sayName: function() {
              alert(this.name);
          }
      };
      
      /*
       * Restoring the constructor in this manner creates a property with
       * [[Enumerable]] set to true.
       * Native constructor properties are not enumerable by default, so 
       * if you're using an ECMAScript 5-compliant JavaScript engine, you
       * may wish to use Object.defineProperty() instead.
       */
       
       function Person() {
       }
       
       Person.prototype = {
           name: "Joe",
           age: 21,
           job: "Web Developer",
           sayName: function() {
               alert(this.name);
           }
       };
       
       // ECMAScirpt 5 only - restore the constructor
       Object.defineProperty(Person.prototype, "constructor", {
           enumerable: false,
           value: Person
       });
```

### Dynamic Nature of Prototypes
Since the process of looking up values on a prototype is a search, changes made to the prototype at any point are immediately reflected on instances, even the instances that existed before the change was made:

```javascript   
    var friend = new Person();
    
    Person.prototype.sayHi = function() {
        alert("hi");
    };
    friend.sayHi();     // "hi" - works
    
    /*
     * When friend.sayHi() is called, the instance is first searched for a
     * property named sayHi(); when it's not found, the search continues to the
     * prototype. 
     * Since the link between the instance and the prototype is simply a pointer,
     * not a copy, the search finds the new sayHi property on the prototype and 
     * returns the function stored there.
     */
```

**Although properties and methods may be added to the prototype at any time, and they are reflected instantly by all object instances, you cannot overwrite the entire prototype and expect the same behavior.**
<br />
The `[[Prototype]]` pointer is assigned when the constructor is called, so changing the prototype to a different object servers the tie between the constructor and the original prototype. Remember: the instance has a pointer to only the prototype, not to the constructor.

```javascript
    function Person() {
    }
    
    var friend = new Person();
    
    Person.prototype = {
        constructor: Person,
        name: "Joe",
        age: 21,
        job: "Web Developer",
        sayName: function() {
            alert(this.name);
        }
    };
    
    friend.sayName();   // error
    
    /* 
     * A new instance of Person is created before the prototype object is overwritten.
     * When friend.sayName() is called, it causes an error, because the prototype that 
     * friend points to doesn't contain a property of that name.
     */
```

Overwriting the prototype on the constructor means that new instances will reference the new prototype while any previously existing object instances still reference the old prototype.

### Native Object Prototypes
The prototype pattern is used to implement all of the native reference types. Each of these has its methods defined on the constructor's prototype. For instance, the `sort()` method can be found on `Array.prototype`, and `substring()` can be found on `String.prototype`.

```javascript
    alert(typeof Array.prototype.sort);     // "function"
    alert(typeof String.prototype.substring);   // "function"
    
    /*
     * It's possible to get references to all of the default methods
     * and to define new methods. Native object prototypes can be modified
     * just like custom object prototypes, so methods can be added at any time.
     */
     
     String.prototype.startsWith = function(text) {
         return this.indexOf(text) == 0;
     };
     
     var msg = "Hello world!";
     alert(msg.startsWith("Hello"));    // true
     
     /* 
      * Although possible, it is not recommended to modify native
      * object prototypes in a production environment.
      */
```

### Problems with Prototypes
* It negates the ability to pass initialization arguments into the constructor, meaning that all instances get the same property values by default.
* The main problem comes with their shared nature.
    * All properties on the prototype are shared among instances, which is ideal for functions.
    * Properties that contain primitive values also tend to work well, as shown in the previous example, where it's possible to hide the `prototype` property by assigning a property of the same name to the instance.
    * The real problem occurs when a property contains a reference value.

```javascript
    function Person() {
    }
    
    Person.prototype = {
        constructor: Person,
        name: "Joe",
        age: 21,
        job: "Web Developer",
        friends: ["Mark", "Charles"],
        sayName: function() {
            alert(this.name);
        }
    };
    
    var person1 = new Person();
    var person2 = new Person();
    
    person1.friends.push("Terry");
    
    alert(person1.friends);     // "Mark,Charles,Terry"
    alert(person2.friends);     // "Mark,Charles,Terry"
    alert(person1.friends === person2.friends);     // true
    
    /* 
     * The person1.friends array is altered by adding another string.
     * Because the friends array exists on Person.prototype, not on 
     * person1, the changes made are also reflected on person2.friends,
     * which points to the same array.
     */
```

## Combination Constructor/Prototype Pattern
The most common way of defining custom types is to combine the constructor and prototype patterns.
* The constructor pattern defines instance properties
* The prototype pattern defines methods and shared properties
* With this approach, each instance ends up with its own copy of the instance properties, but they all share references to methods, conserving memory.
* This pattern allows arguments to be passed into the constructor as well, effectively combining the best parts of each pattern.

```javascript
    function Person(name, age, job) {
        this.name = name;
        this.age = age;
        this.job = job;
        this.friends = ["Mark", "Charles"];
    }
    
    Person.prototype = {
        constructor: Person,
        sayName: function() {
            alert(this.name);
        }
    };
    
    var person1 = new Person("Joe", 21, "Web Developer");
    var person2 = new Person("Qiaohong", 23, "Gamer");
    
    person1.friends.push("Terry");
    
    alert(person1.friends);     // "Mark,Charles,Terry"
    alert(person2.friends);     // "Mark,Charles"
    alert(person1.friends === person2.friends);     // false
    alert(person1.sayName === person2.sayName);     // true
    
    /* 
     * The hybrid constructor/prototype pattern is the most widely
     * used and accepted practice for defining custom reference types
     * in ECMAScript. 
     * This is the default pattern to use for defining reference tyeps.
     */
```

## Dynamic Prototype Pattern
The dynamic prototype pattern seeks to solve this problem by encapsulating all of the information within the constructor while maintaining the benefits of using both a constructor and a prototype by initializing the prototype inside the constructor, but only if it is needed. You determine if the prototype needs to be initialized by checking for the existence of a method taht should be available.

```javascript
    function Person(name, age, job) {
        // properties
        this.name = name;
        this.age = age;
        this.job = job;
        
        // methods
        if (typeof this.sayName != "function") {
            Person.prototype.sayName = function() {
                alert(this.name);
            };
        }
    }
    
    var friend = new Person("Joe", 21, "Web Developer");
    friend.sayName();
    
    /*
     * The section of code below the second comment inside the constructor adds the 
     * sayName() method if it doesn't already exist.
     */
```

You cannot overwrite the prototype using an object literal when using the dynamic prototype pattern. Overwriting a prototype when an instance already exists effectively cuts off that instance from the new prototype.

## Parasitic Constructor Pattern
The parasitic constructor pattern is typically a fallback when the other patterns fail. The basic idea of this pattern is to create a constructor that simply wraps the creation and return of another object while looking like a typical constructor.

```javascript
    function Person(name, age, job) {
        var o = new Object();
        o.name = name;
        o.age = age;
        o.job = job;
        o.sayName = function() {
            alert(this.name);
        };
        return o;
    }
    
    var friend = new Person("Joe", 21, "Web Developer");
    friend.sayName();       // "Joe"
    
    /*
     * This is exactly the same as the factory pattern except that
     * the function is called as a constructor, using the new operator.
     * When a constructor doesn't return a value, it returns the new object
     * instance by default.
     * Adding a return statement at the end of a constructor allows you to
     * override the value that is returned when the constructor is called.
     */
     
     /*
      * This pattern allows you to create constructors for objects that may not be 
      * possible otherwise. For example, you may want to create a special array that 
      * has an extra method.
      */
      
      function SpecialArray() {
          // create the array
          var values = new Array();
          
          // add the values
          values.push.apply(values, arguments);
          
          // assign the method
          values.toPipedString = function() {
              return this.join("|");
          };
          
          // return it
          return values;
      }
      
      var colors = new SpecialArray("red", "blue", "green");
      alert(colors.toPipedString());    // "red|blue|green"
      
      /*
       * There is no relationship between the returned object and 
       * the constructor or the constructor's prototype; the object
       * exists just as if it were created outside of a constructor.
       * Therefore, you cannot rely on the instanceof operator to 
       * indicate the object type.
       * Becuase of these issues, this pattern should not be used when 
       * other patterns work.
       */
```

## Durable Constructor Pattern
Durable objects are best used in secure environements(those that forbid the use of `this` and `new`) or to protect data from the rest of the application(as in mashups). 
* A durable constructor is a constructor that follows a pattern similar to the parasitic constructor pattern, with two differences:
    * instance methods on the created object don't refer to this
    * the constructor is never called using the `new` operator
    
```javascript
    function Person(name, age, job) {
        // create the object to return
        var o = new Object();
        
        // optional: define private variables/functions here
        
        // attach methods
        o.sayName = function() {
            alert(name);
        };
        
        // return the object
        return o;
    }
    
    /*
     * There is no way to access the value of name from the returned obejct.
     * The sayName() method has access to it, but nothing else does.
     */
     
     var friend = Person("Joe", 21, "Web Developer");
     friend.sayName();      // "Joe"
```

As with the parasitic constructor pattern, there is no relationship between the constructor and the object instance, so `instanceof` will not work.
