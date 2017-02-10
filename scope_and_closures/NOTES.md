# Scope & Closures

LHS lookup variables are automatically generated _in the global scope_
if the program is not running in strict mode. This is one of the reasons
it is best practice to always run JS in strict mode.

`ReferenceError` is scope-resolution related.

`TypeError` is an illegal/impossible action request error and only occurs
on successful scope resolution.
