## The temporal dead zone

A variable declared by let or const has a so-called temporal dead zone (TDZ): When entering its scope, it can’t be accessed (got or set) until execution reaches the declaration. Let’s compare the life cycles of var-declared variables (which don’t have TDZs) and let-declared variables (which have TDZs).

### The lifecycle of var-declared variables

*var* variables don’t have temporal dead zones. Their life cycle comprises the following steps:

- When the scope (its surrounding function) of a var variable is entered, storage space (a binding) is created for it. The variable is immediately initialized, by setting it to undefined.
- When the execution within the scope reaches the declaration, the variable is set to the value specified by the initializer (an assignment) – if there is one. If there isn’t, the value of the variable remains undefined.

### The lifecycle of let-declard variables

Variables declared via let have temporal dead zones and their life cycle looks like this:

- When the scope (its surrounding block) of a let variable is entered, storage space (a binding) is created for it. The variable remains uninitialized.
- Getting or setting an uninitialized variable causes a ReferenceError.

When the execution within the scope reaches the declaration, the variable is set to the value specified by the initializer (an assignment) – if there is one. If there isn’t then the value of the variable is set to undefined.
const variables work similarly to let variables, but they must have an initializer (i.e., be set to a value immediately) and can’t be changed.

### Examples

Within a TDZ, an exception is thrown if a variable is got or set:
```
let tmp = true;
if (true) { // enter new scope, TDZ starts
    // Uninitialized binding for `tmp` is created
    console.log(tmp); // ReferenceError

    let tmp; // TDZ ends, `tmp` is initialized with `undefined`
    console.log(tmp); // undefined

    tmp = 123;
    console.log(tmp); // 123
}
console.log(tmp); // true
```

If there is an initializer then the TDZ ends after the initializer was evaluated and the result was assigned to the variable:
```
let foo = console.log(foo); // ReferenceError
```


