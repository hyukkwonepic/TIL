## let
The let variable declaration works similarly to var, except var is function-scoped in JavaScript, while let is block-scoped. Below is an example of functional scoping in JavaScript:
```
(function foo() {
  var a = 10;
  if (true) {
    var a = 20;
    var b = 30;
  };
  console.log(a); // 20
  console.log(b); // 30
})();

```
What we actually want this program to do is within the if statement, we want to create two new variables, a and b, that are locally scoped within that block. We want the variables a and b to only be available within that block and not effect any of the variables outside of that block. We can now fix the above issue by locally scoping the variables to within the block using *let*.

```
(function foo() {
  var a = 10;
  if (true) {
    let a = 20;
    let b = 30;
  }
  console.log(a); // 10
  console.log(b); // ReferenceError: b is not defined
})();
```

## let + for loops

A common issue with var declarations and for loops is that a single binding(storage space) is created for a variable declared in the loop head so usually unexpected results are returned. For example, the code below doesn't output what would be expected.

```
var arr = [];
for (var i = 0; i < 3; i++) {
  arr.push(function() {
    console.log(i);
  });
}

arr[0](); // => 3
arr[1](); // => 3
arr[2](); // => 3
```

At the end of the for loop, each function references the same variable, which is why the value of 3 is returned for each function. Every i in the bodies of the three functions refers to the same binding, which is why they all return the same value.

With the new let variable declaration in ES6, this problem can now be solved the following way:

```
var arr = [];
for (let i = 0; i < 3; i++) {
  arr.push(function() {
    console.log(i);
  });
}

arr[0](); // => 0
arr[1](); // => 1
arr[2](); // => 2
```

This time, each i refers to the binding of one specific iteration and preserves the value that was current at that time. Therefore, each arrow function returns a different value.

## const

The new const declaration is also block-scoped, except the difference between it and let is that cont.

1. must be immediately initialized with a value
2. a different value cannot be assigned to it later on (the variable is immutable)

Here is how you can use const for example:

```
(function () {
  if (true) {
    const name = "Daniel";
    console.log(name); // => Daniel
  }
  console.log(name); // => ReferenceError: name is not defined
})();
```

The following code will produce errors though:

```
const name; // => SyntaxError: Missing initializer in const declaration
```

```
const name = "Daniel";
name = "John" // => TypeError: Assignment to constant variable
```
