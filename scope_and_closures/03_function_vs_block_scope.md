# Chapter 03 - Function vs. Block Scope

Usually we think of functions as a thing we declare and then add code to.
More often we should think the opposite way 'round: write any code you like
and then wrap it in a function declaration to hide it from the global scope.

**The Principle of Least Privilege:** Expose the minimum necessary code and
hide everything else within the function/class/closure/etc.

Putting all of our functions in the global scope violates the Principle
of Least Privilege. Furthermore it is potentially "dangerous" by way
of exposing private implementation details to the public API.

Another benefit of hiding code in functions is avoiding collisions
between variables of the same name but different values.

One of the best examples of a situation where we could run into variable
name collisions is with JavaScript libraries. With dozens, if not hundreds,
of libraries loaded into one project it has become best practice for libraries
to use a single object in the global scope as a namespace (such as the `$`
in jQuery). The library's API is then provided as a set of properties and
methods on that single global object.

The CommonJS module pattern used in Node.js is another example of this,
though solved somewhat differently. We export a public API that is then
imported with `require()` where needed in the project with whatever variable
name you like. This _module_ pattern is the more modern approach to JavaScript
and is considered best practice.

In JavaScript we have both _function declarations_ and _function expressions_
available for our use. If the function must be stored to later be called
in the program use a declaration. In other cases (hiding code in a function
scope, callbacks, etc.) use an expression. Here's an example:

In this case we have 2 `console.log()` calls to variables of the same name but
different values. To prevent a varaible overwrite we are going to wrap one
call in a function scope. Here's the function declaration version:

```javascript
var a = 2;

function foo() {
    var a = 3;
    console.log(a); // 3
}

foo(); // Call foo()

console.log(a); // 2
```

Notice how we had to add both a function declaration and a function call.
We also polluted the global scope with the `foo()` function that will not
be later needed. Here's the same code, but done with a function expression:

```javascript
var a = 2;

(function foo() {
    var a = 3;
    console.log(a); // 3
})();

console.log(a); // 2
```

This pattern is called an _immediately-invoked function expression_ or _IIFE_
(pronounced "iffy"). Note that this prevents a function from being created in
the global scope and, as it is immediately invoked, we don't need to call it
on the next line. This is the preferred method of wrapping code in a function
scope.

A function is a declaration _only if_ the `function` keyword is the first thing
in the line of code. Anything else is an expression.

## Anonymous Functions

Often seen in JavaScript callbacks are _anonymous function expressions_.
Such as:

```javascript
var a = [0, 1, 2, 3, 4];

a.forEach(function(value) {
    console.log(value);
});
```

The function passed into the `.forEach()` method is anonymous.
Though this syntax _is_ valid and rather popular it does have a few drawbacks:

- There's no function name to display in a stack trace.
- You cannot call the function recursivley without using deprecated methods.
- Your code is less legible as the function name should describe what it does.

As such, you're best of naming those inline function expressions; like so:

```javascript
var a = [0, 1, 2, 3, 4];

a.forEach(function printValue(value) {
    console.log(value);
});
```

Not difficult, and it makes for better quality code.

## Immediately Invoked Function Expressions

Though IIFEs are usually an anonymous function, they will likewise benefit
from using a named function.

There are two primary syntax forms for the IIFE.

This:

```javascript
(function iife() {
    // run code here
})();
```

And this:

```javascript
(function iife() {
    // run code here
}());
```

Note the difference in the invoking `()` pair. This makes no change to
functionality and is a stylistic choice.

Note that we _can_ pass the global scope into the IIFE as an argument.
This allows us to have a named value to distingush the global scope from
the local scope in the IIFE.

There is also another IIFE syntax that you may see on occasion
(Used by the UMD project):

```javascript
let myArg = 1;

(function iife(func) {
    func(myArg);
})(function func(param) {
    // define code here
});
```

## Blocks as Scopes

The primary concepts behind block scoping is to keep variable
declarations as close in locality to where they are used as possible.

_On the surface_ JavaScript has no way to implement block scoping.
If we dig a bit there are some ways about it, though.

For instance:

The ES3 spec defines that the varaible declaration in the `catch()` statement
of a `try/catch` block it to be block-scoped to the `catch()` block.
This is used by transpilers for block-scoping functionality.

As of ES6 we have the `let` keyword for varaible declaration. Variables
that are declared with `let` are block-scoped to the surrounding block
(usually a `{ .. }` pair).

One of the best examples of the benefit of using `let` is with a `for() { .. }`
loop. The incrementer variable will be scoped to the loop itself rather than
the surrounding scope as it would be if you used `var`.

ES6 also gives us the `const` keyword for declaring constant values. Those
values are block-scoped as well.

In terms of best practice we can use the `var` and `let` keywords side-by-side
depending on the current need for function or block scoping. `let` is not
an outright replacement for `var`.
