# The Array Type
ECMAScript arrays can hold any type of data in each slot.
<br />
ECMAScript arrays are also dynamically sized.
<br />

First way to create an array:

```javascript
    var color = new Array();    // use the Array constructor
    
    var colors = new Array(10);   // knowing the number of items will be in the array
    
    var colors = new Array("red", "blue", "green");
    
    // It's possible to omit the new operator when using the Array constructor
    var colors = Array(10);
    var names = Array("Joe");
```

Create an array using array literal notation:

```javascript
    var colors = ["red", "blue", "green"];
    var names = [];
    var values = [1, 2,];   // avoid! creates an array with 2 or 3 items
    var options = [,,,,,];    // avoid! creates an array with 5 or 6 items
```

**As with objects, the Array constructor is not called when an array is created using array literal notation.**
<br />
The number of items in an array is stored in the `length` property:

```javascript
    var colors = ["red", "blue", "green"];
    var names = [];
    
    alert(colors.length);   // 3
    alert(names.length);    // 0
```

A unique characteristic of `length` is that it's not read-only. By setting the `length` property, you can easily remove items from or add items to the end of the array.

```javascript
    var colors = ["red", "blue", "green"];
    colors.length = 2;
    alert(colors[2]);   // undefined
    
    var colors = ["red", "blue", "green"];
    colors.length = 4;
    alert(colors[3]);   // undefined
    
    // The length property can also be helpful in adding items to the end of an array
    var colors = ["red", "blue", "green"];
    colors[colors.length] = "black";    // add a color(position 3)
    colors[colors.length] = "brown";    // add another color (position 4)
    
    var colors = ["red", "blue", "green"];
    colors[99] = "black";   // add a color(position 99)
    alert(colors.length);   // 100
```

*Arrays can contain a maximum of 4,294,967,295 items.*

## Detecting Arrays

```javascript
    if (Array.isArray(value)) {
        // do something on the array
    }
```
## Conversion Methods
The `toString()` and `valueOf()` methods return the same value when called on an array. The result is comma-separated string.

```javascript
    var colors = ["red", "blue", "green"];
    alert(colors.toString());   // red,blue,green
    alert(colors.valueOf());    // red,blue,green
    alert(colors);    // red,blue,green
    
    /* 
     * Because alert() expects a string,
     * it calls toString() behind the scenes
     * to get the same result as when
     * toString() is called directly.
     */
```

As for the `toLocaleString()`, when it is called on an array, it creates a comma-delimited string of the array values.<br />
The only difference between this and the two other methods is that `toLocaleString()` calls each item's `toLocaleString()` instead of `toString()` to get its string value.

```javascript
    var person1 = {
        toLocaleString: function() {
            return "Joe";
        },
        
        toString : function() {
            return "Qiaohong";
        }
    };
    
    var person2 = {
        toLocaleString: function() {
            return "Bruce";
        },
        
        toString: function() {
            return "Lee";
        }
    };
    
    var people = [person1, person2];
    alert(people);    // Qiaohong,Lee
    alert(people.toString());   // Qiaohong,Lee
    alert(people.toLocaleString());   // Joe, Bruce
```

It's possible to construct a string with a different separator using the `join()` method, which accepts one argument:

```javascript
    var colors = ["red", "green", "blue"];
    alert(colors.join(","));    // red,green,blue
    alert(colors.join("||"));   // red||green||blue
```

**If an item in the array is `null` or `undefined`, it is represeneted by an empty string.**

## Stack Methods
An array object can act just like a stack, which is one of a group of data structures that restrict the insertion and removal of items. A stack is referred to as a *last-in-first-out*(LIFO) structure, meaning that the most recently added item is the first one removed.
* Insertion(called a *push*) - `push()`
* Removal(called a *pop*) - `pop()`
* both of them occur at only one point: the top of the stack
<br />
The `push()` method accepts any number of arguments and adds them to the end of the array, returning the array's new length:

```javascript
    var colors = new Array();
    var count = colors.push("red", "green");
    alert(count);   // 2
    
    count = colors.push("black");
    alert(count);   // 3
    
    var item = colors.pop();
    alert(item);    // "black"
    alert(colors.length);   // 2
```

**`push()` and `pop()` are default methods on arrays.**
Another example:

```javascript
    var colors = ["red", "blue"];
    colors.push("brown");
    colors[3] = "black";
    alert(colors.length);   // 4
    
    var item = colors.pop();
    alert(item);    // "black"
```

## Queue Methods
Queues restrict access in a *first-in-first-out*(FIFO) data structure. A queue adds items to the end of a list and retrieves items from the front of the list.
* `push()` method adds items to the end of an array
* `shift()` method retrieves the first item in the array, and returns it, decrementing the length of the array by one

```javascript
    var colors = new Array();
    var count = colors.push("red", "green");
    alert(count);   // 2
    
    count = colors.push("black");
    alert(count);   // 3
    
    var item = colors.shift();    // returns the first item of the array
    alert(item);    // "red"
    alert(colors.length);   // 2
```

ECMAScript provides an `unshift()` method for arrays, which does the opposite of `shift()`:
* adds any number of items to the front of an array and returns the new array length

```javascript
    var colors = new Array();
    var count = colors.unshift("red", "green");   // push two items
    alert(count);   // 2
    // now the array is ["red", "green"]
    
    count = colors.unshift("black");
    // now the array is ["black", "red", "green"]
    alert(count);   // 3
    
    var item = colors.pop();
    alert(item);    // "green"
    // now the array is ["black", "red"]
    alert(colors.length);   // 2
```
## Reordering Methods
* `reverse()` simply reverses the order of items in an array

```javascript
    var values = [1, 2, 3, 4, 5];
    values.reverse();
    alert(values);    // 5,4,3,2,1
```
* `sort()` calls the String() casting function on every item and then compares the strings to determine the correct order.
  * This occurs even if all items in an array are numbers

```javascript
    var values = [0, 1, 5, 10, 15];
    values.sort();
    alert(values);    // 0,1,10,15,5
```

`sort()` method allows you to pass in a *comparison function* that indicates which value should come before which.

```javascript
    function compare(num1, num2) {
        if (num1 < num2) {
            return -1;
        } else if (num1 > num2) {
            return 1;
        } else {
            return 0;
        }
    }
    
    var values = [0, 1, 5, 10, 15];
    values.sort(compare);
    alert(values);    // 0,1,5,10,15
    
    // Or we can produce results in descending order by switch the return values
    function compare(num1, num2) {
        if (num1 < num2) {
            return 1;
        } else if (num1 > num2) {
            return -1;
        } else {
            return 0;
        }
    }
    
    var values = [0, 1, 5, 10, 15];
    values.sort(compare);
    alert(values);    // 15,10,5,1,0
```

**Both `reverse()` and `sort()` return a reference to the array on which they were applied.** <br />
Here's a much simpler version of the comparison function can be used with numeric types, and objects whose `valueOf()` method returns numeric values(such as the `Date` object).

```javascript
    function compare(value1, value2) {
        return value2 - value1;   // returning a number less than zero, zero, or a number greater than zero
    }
```

## Manipulation Methods
`concat()` method allows you to create a new array based on all of the items in the current array.
* begins by creating a copy of the array and then appending the method arguments to the end and returning the newly constructed array.
* no arguments are passed in, simply clones the array and returns it
* one or more arrays are passed in, appends each item in these arrays to the end of the result
* If the values are not arrays, simply appended to the end of the resulting array.

```javascript
    var colors = ["red", "green", "blue"];
    var colors2 = colors.concat("yellow", ["black", "brown"]);
    
    alert(colors);    // red,green,blue
    alert(colors2);   // red,green,blue,yellow,black,brown
```

`slice()` method, creates an array that contains one or more items already contained in an array.
* may accept one or two arguments: the starting and stopping positions of the items to return
* If only one argument, the method returns all items between that position and the end of the array
* If two arguments, the method returns all items between the start position and the end position, not including the item in the end position.
* This operation does not affect the original array.

```javascript
    var colors = ["red", "green", "blue", "yellow", "purple"];
    var colors2 = colors.slice(1);
    var colors3 = colors.slice(1,4);
    
    alert(colors2);   // green,blue,yellow,purple
    alert(colors3);   // green,blue,yellow
```

**Calling `slice(-2, -1)` on an array with five items is the same as calling `slice(3, 4)`**.
<br />
`splice()` method:
* main purpose is to insert items into the middle of an array
* Three distinct ways of using this method:
  * Deletion - Two arguments: the position of the first item to delete and the number of items to delete
    * `splice(0, 2)` deletes the first two items
  * Insertion - Three or more arguments: the starting position, 0(the number of items to delete), and the item to insert
    * `splice(2, 0, "red", "green")` inserts the strings "red" and "green" into the array at position 2
  * Replacement - Items can be inserted into a specific position while simultaneously deleting items, if you specify three arguments: the starting position, the number of items to delete, and any number of items to insert
    * `splice(2, 1, "red", "green")` deletes one item at position 2 and then inserts the string "red" and "green" into the array at position 2
* This method always returns an array that contains any items that were removed from the array(or an empty array)

```javascript
    var colors = ["red", "green", "blue"];
    var removed = colors.splice(0, 1);
    alert(colors);    // green,blue (The length of the array is 2 now)
    alert(removed);   // red - one item array
    
    removed = colors.splice(1, 0, "yellow", "orange");
    alert(colors);    // green,yellow,orange,blue
    alert(removed);   // empty array
```

## Location Methods
ECMAScript 5 adds two item location methods to array instances: `indexOf()` and `lastIndexOf()`
* Each of these methods accepts two arguments: the item to look for and an optional index from which to start looking
* The methods each return the position of the item in the array or -1 if the item isn't in the array
* An identity comparison is used when comparing the first argument to each item in the array, meaning that the items must be strictly equal as if compared using `===`

```javascript
    var numbers = [1,2,3,4,5,4,3,2,1];
    
    alert(numbers.indexOf(4));    // 3
    alert(numbers.lastIndexOf(4));    // 5
    
    alert(numbers.indexOf(4, 4));   // 5
    alert(numbers.lastIndexOf(4, 4));   // 3
    
    var person = {name: "Joe" };
    var people = [{name : "Joe" }];
    var morePeople = [person];
    alert(people.indexOf(person));    // -1
    alert(morePeople.indexOf(person));    // 0
    
```

## Iterative Methods
ECMAScript 5 defines five iterative methods for arrays, each of them accepts two arguments: a function to run on each item and an optional scope object in which to run the function, affecting the value of `this`. The function passed into one of these methods will receive three arguments: the array item value, the position of the item in the array, and the array object itself.
* `every()` - runs the given function on every item in the array and returns `true` if the function returns `true` for every item
* `filter()` - runs the given function on every item in the array and returns an array of all items for which the function returns `true`
* `forEach()` - has no return value
* `map()` - returns the result of each function call in an array
* `some()` - returns `true` if the function returns `true` for any one item
<br />
**These methods do not change the values contained in the array**

Example for `every()` and `some()`:

```javascript
    var numbers = [1,2,3,4,5,4,3,2,1];
    var everyResult = numbers.every(function(item, index, array){
        return (item > 2);
    });
    
    alert(everyResult);   // false
    
    var someResult = numbers.some(function(item, index, array){
        return (item > 2);
    });
    
    alert(someResult);    // true
```

Example for `filter()`:

```javascript
    var numbers = [1,2,3,4,5,4,3,2,1];
    
    var filterResult = numbers.filter(function(item, index, array){
        return (item > 2);
    });
    
    alert(filterResult);    // 3,4,5,4,3
    
    // This method is very helpful when querying an array for all items matching some criteria
```

Example for `map()`:


```javascript
    var numbers = [1,2,3,4,5,4,3,2,1];
    
    var mapResult = numbers.map(function(item, index, array){
        return item * 2;
    });
    
    alert(mapResult);   // 2,4,6,8,10,8,6,4,2
```

Example for `forEach()`:

```javascript
    var numbers = [1,2,3,4,5,4,3,2,1];
    
    numbers.forEach(function(item, index, array){
        // do something here
    });
```

## Reduction Methods
ECMAScript 5 introduced two reduction methods for arrays: `reduce()` and `reduceRight()`
* Both methods iterate over all items in the array and build up a value that is ultimately returned.
* The `reduce()` method does this starting at the first item and traveling toward the last
* The `reduceRight()` starts at the last and travels towards the first
* Both methods accept two arguments: a function to call on each item and an optional initial value upon which the reduction is based.
* The function passed into these methods accepts four arguments:
  * the previous value
  * the current value
  * the item's index
  * the array object
* Any value returned from the function is automatically passed in as the first argument for the next item
* The first iteration occurs on the second item in the array

Example for `reduce()`:

```javascript
    var values = [1,2,3,4,5];
    var sum = values.reduce(function(prev, cur, index, array){
        return prev + cur;
    });
    alert(sum);   // 15
    
    /*
     * 1st iteration: 1 + 2
     * 2nd iteration: 3 + 3
     * 3rd iteration: 6 + 4
     * 4th iteration: 10 + 5
     * output: 15
     */
```

Example for `reduceRight()`:

```javascript
    var values = [1,2,3,4,5];
    var sum = values.reduceRight(function(prev, cur, index, array){
        return prev + cur;
    });
    alert(sum);   // 15
    
    /*
     * 1st iteration: 5 + 4
     * 2nd iteration: 9 + 3
     * 3rd iteration: 12 + 2
     * 4th iteration: 14 + 1
     * outputs: 15
     */
```
