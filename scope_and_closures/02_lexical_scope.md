# Chapter 02 - Lexical Scope

The JS engine will stop climbing scope layers once it finds the value it's
looking for. For example:

```javascript
var a = 4;

function foo() {
    var a = 2;
    var b = 20;

    function baz(c) {
        console.log(a, b, c);
    }

    baz(b + a);
}

foo();
```

In this code the `console.log()` statement will print 2, 20, 22.
NOT 4, 20, 24. The engine will find the `var a = 2` within the scope of `foo()`
and never contine to the global scope with `var a = 4`.

Lexical scoping is the default behavior of JavaScript and strictly adhering
to it's use is the best practice. However, there are two mechanisims that
allow us to cheat on lexical scope (though neither _shoud_ be used):

- `eval()`: Used to run a dynamically-generated string of JavaScript.
- `with()`: Shorthand method to access obeject properties. _Now depreciated_.

`eval()` can alter the scope by the dynamic parsing and lexing of code
_during runtime_.  This can result in some rather confusing bugs and errors.
Use of this function is highly restricted in strict mode.

`with()` will end up creating a global variable (per the rules of LHS access)
if you try to access an object property that does not exist.  Use of the
function is completely disallowed in strict mode.