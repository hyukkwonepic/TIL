## Computed property names

Starting with ECMAScript 2015, the object initializer syntax also supports computed property names. 
That allows you to put an expression in brackets [], that will be computed as the property name. This is symmetrical to the bracket notation of the property accessor syntax, which you might have used to read and set properties already. Now you can use the same syntax in object literals, too:

```
// Computed property names (ES2015)
var i = 0;
var a = {
  ['foo' + ++i]: i,
  ['foo' + ++i]: i,
  ['foo' + ++i]: i
};

console.log(a.foo1); // 1
console.log(a.foo2); // 2
console.log(a.foo3); // 3

var param = 'size';
var config = {
  [param]: 12,
  ['mobile' + param.charAt(0).toUpperCase() + param.slice(1)]: 4
};

console.log(config); // {size: 12, mobileSize: 4}
```
