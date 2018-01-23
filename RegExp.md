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
or a combination of simple and special characters, such as `/ab*c/` or `/Chapter (\d+)\.\d*/`.
The last example includes parentheses which are used as a memory device. 
The match made with this part of the pattern is remembered for later use.

### Using simple patterns
Simple patterns are constructed of characters for which you want to find a direct match.
* `/abc/` matches character combinations in strings only when exactly the characters 'abc' occur together and in that order.

### Using special characters
When you want to find one or more b's, or to find white space, the pattern includes special characters.
* `/ab*c/` matches any character combination in which a single 'a' is followed by zero or more 'b's and then immediately followed by 'c'.

**Special characters in regular expressions:**

|**Character**|**Meaning**|
|---|---|
|`\`||
|`^`|Matches beginning of input. If the multiline flag is set to true, also matches immediately after a line break character. <br />For example, `/^A/` does not match the 'A' in "an A", but does match the 'A' in "An E".|
|`$`|Matches end of input. If the multiline flag is set to true, also matches immediately before a line break character. <br />For example, `/t$/` does not match the 't' in "eater", but does match it in "eat".|
|`*`|Matches the preceding expression 0 or more times. Equivalent to {0,}. <br /> For example, `/bo*/` matches 'boooo' in "A ghost boooped" and 'b' in "A bird warbled" but nothing in "A goat grunted".|
|`+`|Matches the preceding expression 1 or more times. Equivalent to {1,}. <br /> For example, `/a+/` matches the 'a' in "candy" and all the a's in "caaaaaandy", but nothing in "cndy".|
|`?`|Matches the preceding expression 0 or 1 time. Equivalent to {0,1}. <br />For example, `/e?le?/` matches the 'el' in "angel" and the 'le' in "angle" and also the 'l' in "oslo". <br /> If used immediately after any of the quantifiers \*, \+, ?, or {}, makes the quantifier non-greedy(matching the fewest possible characters), as opposed to the default, which is greddy(matching as many characters as possible). <br /> For example, applying `/\d+/` to "123abc" matches "123". But applying `/\d+?/` to that same string matches only the "1".|
|`.`|(The decimal point)matches any single character except the newline character. <br /> For example, `/.n/` matches 'an' and 'on' in "nay, an apple is on the tree", but not 'nay'.|
|`(x)`|Matches 'x' and remembers the match, as the following example shows. The parentheses are called *capturing parentheses*. <br /> The '`(foo)`' and '`(bar)`' in the pattern `/(foo) (bar) \1 \2` match and remember the first two words in the string "foo bar foo bar". The `\1` and `\2` in the pattern match the string's last two words. Note that `\1`, `\2`, ..., `\n` are used in the matching part of the regex. In the replacement part of a regex the syntax `$1`, `$2`,...,`$n` must be used, e.g.: `'bar foo'.replace(/(...) (...)/, '$2 $1')`. `$&` means the whole matched string.|
|`(?:x)`|Matches 'x' but does not remember the match. The parentheses are called *non-capturing parentheses*, and let you define subexpressions for regular expression operators to work with. Consider the sample expression `/(?:foo){1,2}/`. If the expression was `/foo{1,2}/`, the `{1,2}` characters would apply only to the last 'o' in 'foo' **Meaning the second 'o' can appeaer at least once and at most twice**. With the non-capturing parentheses, the {1,2} applies to the entire word 'foo'. **This mean matching at least once of 'foo' and at most twice of 'foo'**|
|`x(?=y)`||
|`x(?!y)`||
|`x\|y`||
|`{n}`||
|`{n,}`||
|`{n,m}`||
|`[xyz]`||
|`[^xyz]`||
|`[\b]`||
|`\b`||
|`\B`||
|`\cX`||
|`\d`||
|`\D`||
|`\f`||
|`\n`||
|`\r`||
|`\s`||
|`\S`||
|`\t`||
|`\v`||
|`\w`||
|`\W`||
|`\n`|Where n is a positive integer, a back reference to the last substring matching the n parenthetical in the regular expression(counting left parentheses). <br /> For example, `/apple(,)\sorange\1` matches 'apple, orange' in "apple, orange, cherry, peach."|
|`\0`||
|`\xhh`||
|`\uhhhh`||
|`\u{hhhh}`||






### Using parentheses
Parentheses around any part of the regular expression pattern causes that part of the matched substring to be remembered. Once remembered, the substring can be recalled for other use.
<br />
For example, the pattern `/Chapter (\d+)\.\d*/` illustrates additional escaped and special characters and indicates that part of the pattern should be remembered. It matches precisely the caracters 'Chapter' followed by one or more numeric characters(`\d` means any numeric character and `+` means 1 or more times), followed by a decimal point(which in itself is a special character; preceding the decimal point with \\ means the pattern must look for the literal character '.'), followed by any numeric character 0 or more times(`\d` means numeric character, `*` means 0 or more times). In addition, parentheses are used to remember the first matched numeric characters.
<br />
This pattern is found in "Open Chapter 4.3, paragraph 6" and '4' is remembered. The pattern is not found in "Chapter 3 and 4", because that string does not have a period after the '3'.
<br />
To match a substring without causing the matched part to be remembered, within the parentheses preface the pattern with `?:`. For example, `(?:\d+)` matches one or more numeric characters but does not remember the matched characters.

## Working with regular expressions

### Using parenthesized substring matches

### Advanced searching with flags

## Examples

### Changing the order in an input string

### Using special characters to verify input
