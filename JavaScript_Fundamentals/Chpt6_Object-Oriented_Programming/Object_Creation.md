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

## The Prototype Pattern

## Combination Constructor/Prototype Pattern

## Dynamic Prototype Pattern

## Parasitic Constructor Pattern

## Durable Constructor Pattern
