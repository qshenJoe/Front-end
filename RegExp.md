# Regular Expressions

## Creating a regular expression
You construct a regular expression in one of two ways:
* Using a regular expression literal, which consists of a pattern enclosed between slashes:
  * Regular expression literals provide compilation of the regular expression when the script is loaded. 
  If the regular expression remains constant, using this can improve performance.
  
```javascript
    var re = /ab+c/;
```

* Calling the constructor function of the `RegExp` object:
  * Using the constructor function provides runtime compilation of the regular expression.
  Use the constructor function when you know the regular expression pattern will be changing,
  or you don't know the pattern and are getting it from another source, such as user input.
  
```javascript
    var re = new RegExp("ab+c");
```

## Writing a regular expression pattern
A regular expression pattern is composed of simple characters, such as `/abc/`,
or a combination of simple and special characters, such as `/ab*c/` or `/Chapter(\d+)\.\d*/`.
The last example includes parentheses which are used as a memory device. 
The match made with this part of the pattern is remembered for later use.

### Using simple patterns

### Using special characters

### Using parentheses

## Working with regular expressions

### Using parenthesized substring matches

### Advanced searching with flags

## Examples

### Changing the order in an input string

### Using special characters to verify input
