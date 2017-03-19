## ES6 template literals

JavaScript ES6 introduces a simple way to perform string interpolation/concatenation. Before, you had to combine strings and variables using the + operator which would convert certain variables to strings. Below is an example of how variables are commonly concatenated with strings: 
```
var name = 'Alex';
var action = 'coding';
var time = 3;

var str = name + ' is currently ' + action + ' for ' + time + ' hours.';
console.log(str); // => Alex is currently coding for 3 hours.
```

This method sometimes gave rise to some errors or bugs, but now with ES6 we have template literals, which looks like the following:

```
var name = 'Alex';
var action = 'coding';
var time = 3;

var str = `${name} is currently ${action} for ${time} hours.`;
console.log(str); // => Alex is currently coding for 3 hours.
```

The way to create template literals are to enclose your string within back-ticks (` `) instead of quotes (" ") and to include variables and expressions within the string you use the following syntax: *${expression}*

You can also have strings span multiple lines now with template literals:

```
var str = `Hello world
this string spans
multiple lines`;
```
