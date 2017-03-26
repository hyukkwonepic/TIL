## IIFE - Immediately-invoked function expression

IIFE is a JS idiom which produces a lexical scope using JavaScript's function scoping.

It can be used to avoid variable hoisting from within blocks, protect against polutting the global environment and simultaneously allow public access to methods while retaining privacy for variables defined within the function. 

This concept has been referred to as a self-executing anonymous funciton.

usage:
```
(function () {
  // all of your code here
  var foo = function() {};
  window.onload = foo;
  // ...
})();
// foo is unreachable here
```

The first pair of parentheses *(*function(){...}*)* turns the code within (in this case, a function) into an expression, and the second pair of parentheses (function(){...})*()* calls the function that results from that evaluated expression.

This pattern is often used when trying to avoid polluting the global namespace, because all the variables used inside the IIFE (like in any other normal function) are not visible outside its scope.


### Parse vs Execution
The function is executed right after it's created, not after it is parsed. The entire script block is parsed before any code in it is executed. Also, parsing code doesn't automatically mean that it's executed, if for example the IIFE is inside a function then it won't be executed until the function is called.
