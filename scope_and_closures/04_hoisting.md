# Hoisting

The JavaScript compiler views an assignment such as `var a = 2;` as two
separate statements: `var a;` and `a = 2;`. This can have some interesting
results. Take the following code:

```javascript
console.log(a);

var a = 2;
```

This will _not_ result in a `ReferenceError` as you might expect. The JS
compiler will hoist the `var a` statement and run the code like this:

```javascript
var a;

console.log(a);

a = 2;
```

This means that the `console.log(a);` statement will actually output `undefined`.

**NOTE:** Only declarations are hoisted. Assignments and logic are left in
place by the compiler.

Hoisting is on a per-scope basis. The compiler will not hoist declarations
outside of their current scope.

Function declarations are hoisted; function expressions are not.

Function declarations are hoisted _before_ variables.

Duplicate `var` declarations are ignored.

Later duplicate function declarations will override earlier ones.

Duplicate function declarations in the same scope are a _really bad idea_.

Function declarations inside of blocks will _typically_ hoist to the
enclosing scope, but this is not reliabile behavior. As a result you
should avoid declaring functions inside of blocks.
