# The RegExp Type
Regular expression syntax:

```javascript
    var expression = /pattern/flags;
```

Each expression can have zero or more flags indicating how the expression should behave:
* g - indicates global mode, meaning the pattern will be applied to all of the string instead of stopping after the first match is found
* i - indicates case-insensitive mode, meaning the case of the pattern and the string are ignored when determining matches
* m - indicates multiline mode, meaning the pattern will continue looking for matches after reaching the end of one line of text

A regular expression is created using a combination of pattern and these flags to produce different results:

```javascript
    // match all instances of "at" in a string
    var pattern1 = /at/g;
    
    // match the first instance of "bat" or "cat", regardless of case
    var pattern2 = /[bc]at/i;
    
    // match all three-character combinations ending with "at", regardless of case
    var pattern3 = /.at/gi;
```

**All metacharacters must be escaped when used as part of the pattern**:
<br />
( \[ { \ ^ $ | } ] ) ? * + .

```javascript
    // match the first instance of "bat" or "cat", regardless of case
    var pattern1 = /[bc]at/i;
    
    // match the first instance of "[bc]at", regardless of case
    var pattern2 = /\[bc\]at/i;
    
    // match all three-character combinations ending with "at", regardless of caes
    var pattern3 = /.at/gi;
    
    // match all instances of ".at", regardless of case
    var pattern4 = /\.at/gi;
    
    /*
     * Any regular expression that can be defined using literal syntax can also be defined using the constructor
     */
     var pattern1 = /[bc]at/i;
     
     var pattern2 = new RegExp("[bc]at", "i");
```

There are some instances in which you need to double-escape characters.

|**LITERAL PATTERN**|**STRING EQUIVALENT**|
|---|---|
|/\\[bc\\]at/|"\\\\[bc\\\\]at"|
|/\\.at/|"\\\\.at"|
|/name\\/age/|"name\\\\/age"|
|/\\d.\\d{1,2}/|"\\\\d.\\\\d{1,2}"|
|/\\w\\\\hello\\\\123/|"\\\\w\\\\\\\\hello\\\\\\\\123"|

```javascript
    /*
     * Creating a regular expression using a literal is not
     * exactly the same as creating a regular expression using
     * the RegExp constructor.
     * In ECMAScript 3, regular-expression literals always
     * share the same RegExp instance, while creating a new 
     * RegExp via constructor always results in a new instance.
     */
     
     var re = null,
         i;
     for (i = 0; i < 10; i++) {
         re = /cat/g;  // Instance properties are not reset, so calling test() fails every other time through the loop
         re.test("catastrophe");  // the "cat" is found in the first call to test(), but the second call begins its search from index 3(the end of last match)
         // since the end of the string is found, the subsequent call to test() starts at the beginning again
     }
     
     for (i = 0; i < 10; i++) {
         re = new RegExp("cat", "g");   // the RegExp constructor create the regular expression each time through the loop
         re.test("catastrophe");
     }
     
     // ECMAScript 5 clarifies the behavior of regular-expression literals by explicitly stating that regular-expression literals must create new instances of RegExp as if the RegExp constructor were called directly
```

## RegExp Instance Properties
Each instance of `RegExp` has the following properties:
* `global` - A Boolean value indicating whether the `g` flag has been set
* `ignoreCase` - A Boolean value indicating whether the `i` flag has been set
* `lastIndex` - An integer indicating the character position where the next match will be attempted in the source string. This value always begins as 0
* `multiline` - A Boolean value indicating whether the `m` flag has been set
* `source` - The string source of the regular expression. This is always returned as if specified in literal form(without opening and closing slashes) rather than a string pattern as passed into the constructor

```javascript
    var pattern1 = /\[bc\]at/i;
    
    alert(pattern1.global);   // false
    alert(pattern1.ignoreCase);   // true
    alert(pattern1.multiline);    // false
    alert(pattern1.lastIndex);    // 0
    alert(pattern1.source);   // "\[bc\]at"
    
    var pattern2 = new RegExp("\\[bc\\]at", "i");
    
    alert(pattern2.global);   // false
    alert(pattern2.ignoreCase);   // true
    alert(pattern2.multiline);    // false
    alert(pattern2.lastIndex);    // 0
    alert(pattern2.source);   // "\[bc\]at"
```

## RegExp Instance Methods
The primary method of a `RegExp` object is `exec()`, which is intended for use with capturing groups.
* It accepts a single argument, which is the string on which to apply the pattern
* It returns an array of information about the first match or `null` if no match was found.
* The returned array, though an instance of `Array`, contains two additional properties:
  * `index` - the location in the string where the pattern was found
  * `input` - the string that the expression was run against
* In the array, the first item is the string that matches the entire pattern
* Any additional items represent captured groups inside the expression

```javascript
    var text = "mom and dad and baby";
    var pattern = /mom( and dad( and baby)?)?/gi;
    
    var matches = pattern.exec(text);
    alert(matches.index);   // 0 because the entire text matches
    alert(matches.input);   // "mom and dad and baby"
    alert(matches[0]);    // "mom and dad and baby"
    alert(matches[1]);    // " and dad and baby"
    alert(matches[2]);    // " and baby"
    
    /* 
     * This execution is returning an array
     * in which the index 0 contains the entire matched string
     * and index 1 to n contains 1st to nth capturing groups
     */
```

The `exec()` method returns information about one match at a time even if the pattern is global. When the global flag is not specified, calling `exec()` on the same string multiple times will always return information about the first match. With the `g` flag set on the pattern, each call to `exec()` moves further into the string looking for matches:

```javascript
    var text = "cat, bat, sat, fat";
    var pattern1 = /.at/;
    
    var matches = pattern1.exec(text);
    alert(matches.index);       // 0
    alert(matches[0]);      // cat
    alert(pattern1.lastIndex);      // 0
    
    matches = pattern1.exec(text);
    alert(matches.index);       // 0
    alert(matches[0]);      // cat
    alert(pattern1.lastInex);       //0
    
    var pattern2 = /.at/g;
    
    var matches = pattern2.exec(text);
    alert(matches.index);       // 0
    alert(matches[0]);      // cat
    alert(pattern2.lastIndex);   // 0
    
    matches = pattern2.exec(text);
    alert(matches.index);   // 5
    alert(matches[0]);      // bat
    alert(pattern2.lastIndex);  // 8
```

Another method of regular expression is `test()`, which accepts a string argument and returns `true` if the pattern matches the argument and `false` if it does not. This is useful when you want to know if a pattern is matched, but you have no need for the actual matched text.

```javascript
    var text = "000-00-0000";
    var pattern = /\d{3}-\d{2}-\d{4}/;
    
    if (pattern.test(text)) {
        alert("The pattern was matched.");
    }
    
    /* 
     * This functionality is often used for validating user input,
     * when you care only if the input is valid,
     * not necessarily why it's invalid.
     */
```

The inherited methods of `toLocaleString()` and `toString()` each return the literal representation of the regular expression, regardless of how it was created.

```javascript
    var pattern = new RegExp("\\[bc\\]at", "gi");
    alert(pattern.toString());      // \/[bc\]at/gi
    alert(pattern.toLocaleString());    // /\[bc\]at/gi
    
    /* 
     * Even though the pattern in this example is created
     * using the RegExp constructor, the toLocaleString()
     * and toString() methods return the pattern as if 
     * it were specified in literal format.
     */
     
     /*
      * The valueOf() method for a regular expression returns the regular expression itself
      */
```
## RegExp Constructor Properties

## Pattern Limitations
