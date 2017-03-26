## Pitfall: const does not make the value immutable 

const only means that a variable always has the same value, but it does not mean that the value itself is or becomes immutable. For example, obj is a constant, but the value it points to is mutable – we can add a property to it:

```
const obj = {};
obj.prop = 123;
console.log(obj.prop); // 123
```
We cannot, however, assign a different value to obj:
```
obj = {}; // TypeError
```
If you want the value of obj to be immutable, you have to take care of it, yourself. For example, by freezing it:
```
const obj = Object.freeze({});
obj.prop = 123; // TypeError
```

### Pitfall: Object.freeze() is shallow 

Keep in mind that Object.freeze() is shallow, it only freezes the properties of its argument, not the objects stored in its properties. For example, the object obj is frozen:

```
> const obj = Object.freeze({ foo: {} });
> obj.bar = 123
TypeError: Can't add property bar, object is not extensible
> obj.foo = {}
TypeError: Cannot assign to read only property 'foo' of #<Object>
```
But the object obj.foo is not.
```
> obj.foo.qux = 'abc';
> obj.foo.qux
'abc'
```
## const in loop bodies 

Once a const variable has been created, it can’t be changed. But that doesn’t mean that you can’t re-enter its scope and start fresh, with a new value. For example, via a loop:

```
function logArgs(...args) {
    for (const [index, elem] of args.entries()) { // (A)
        const message = index + '. ' + elem; // (B)
        console.log(message);
    }
}

logArgs('Hello', 'everyone');

// Output:
// 0. Hello
// 1. everyone
```
There are two const declarations in this code, in line A and in line B. And during each loop iteration, their constants have different values.
