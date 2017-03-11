## String is immutable, array is mutable.

In JavaScript, String values are immutable, which means that they cannot be altered once created.

For example, the following code:

```
var myStr = "Bob";
myStr[0] = "J";
```
cannot change the value of myStr to "Job", because the contents of myStr cannot be altered. Note that this does not mean that myStr cannot be changed, just that the individual characters of a string literal cannot be changed. The only way to change myStr would be to assign it with a new string, like this:
```
var myStr = "Bob";
myStr = "Job";
```

Unlike strings, the entries of arrays are mutable and can be changed freely.

```
var ourArray = [3,2,1];
ourArray[0] = 1; // equals [1,2,1]
```
