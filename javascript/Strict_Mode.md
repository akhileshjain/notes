## Strict Mode
JavaScript Strict Mode is a new feature in ECMAScript 5 that enables you to code a program, or a method, in a “strict” operating context. It is a way to opt into a restricted variant of JavaScript. Hence, implicitly opting-out of **sloppy mode**. This strict context prevents from taking some actions. As a result, it throws more exceptions.

The strict mode in JavaScript does not allow following things:

- Use of undefined variables
- Use of reserved keywords as variable or function name
- Duplicate properties of an object
- Duplicate parameters of function
- Assign values to read-only properties
- Modifying arguments object
- Octal numeric literals
- with statement
- eval function to create a variable

### Strict Mode can be enforced in two ways
- It can be defined in global scope for the entire script.
- It can be defined in individual functions.

*Note* - strict mode does not work with block statements enclosed in {} braces.

## Using strict mode for modules
ECMAScript 2015 introduced JavaScript modules. Therefore, a 3rd way of invoking strict mode came with it. Here, the entire contents of JavaScript modules are automatically in strict mode, with no statement needed to initiate it.
    
    function strictFunc() {
        // because this is a module, it is strict by default
        console.log(“ the contents inside are   automatically in strict mode”);
    }                                    
    export default strictFunc;

Here, the module defined as strictFunc() is automatically in strict mode. It is not needed to use “use strict” statement.