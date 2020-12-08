# Hoisting

Simply read this article - [Link](https://www.tutorialsteacher.com/javascript/javascript-hoisting). 

This gives a clear and concise explanation of hoisting.



#### What is the difference between Function Declaration and Function Expression?

Function Declaration load before any code is executed while Function expressions load only when the interpreter reaches that line of code. Similar to the var statement, function declaration are hoisted to the top of other code.
Function expressions aren't hoisted, which allows them to retain a copy of the local variables from the scope where they were

Function Expression example:
    
    alert(foo()) // ERROR: foo wasn't loaded yet
    var foo = function() { return 5; }

Function Declaration example:

    alert(foo()) // Alerts 5
    function foo() {return 5;}