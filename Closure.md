## Closure

Before learning about Closure, one needs to learn about Scope.

In JavaScript, each function gets its own scope. Scope is basically a collection of variables as well as rules for how those variables are accessed by name. Only code inside that function can access that function's scope.

A variable name has to be unique within the same scope. 

A **closure** is a feature in JavaScript where an inner function has access to the outer (enclosing) function’s variables — *a scope chain*.

The closure has **three** scope chains:
- it has access to its own scope — variables defined between its curly brackets.
- it has access to the outer function's variables.
- it has access to the global variables.


        function outer() {
           var b = 10;
           function inner() {
           var a = 20; 
           console.log(a+b);
          }
         return inner;
        }

Here we have two functions:
 - an outer function `outer` which has a variable `b`, and returns the inner function.
 - an inner function `inner` which has its variable called `a`, and accesses an outer variable `b`, within its function body.

 
