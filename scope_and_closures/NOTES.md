# Scope & Closures

## 01 - What is Scope

Left-hand side assignments are for variable _assignment_.
Right-hand side assignments are for varaible _retreival_.

LHS lookup variables are automatically generated _in the global scope_
if the program _is not running in strict mode_. This is one of the reasons
it is best practice to always run JS in strict mode.

`ReferenceError` is scope-resolution related.

`TypeError` is an illegal/impossible action request error and only occurs
on successful scope resolution.
