There are two types of data types in JS.
 1. Primitive Data Types
   - Stored directly in the location the variable accesses
   - Stored on the stack.
   - **Examples** - Strings, Number, Boolean, Null, Undefined, Symbols (ES6).

   Learning key - UBS NNS

2. Reference Data Types
  - Accessed by reference.
  - Objects that are stored on the heap.
  - A pointer to the location in memory.
  - **Examples** - Arrays, Object Literals, Functions, Dates, Anything else.. 
 
## Did you know?

- Only primitive type which has a type of object is **null**. This is an anomaly from the first version of JS**

    `typeof(null) -> object`

    In the first implementation of JavaScript, JavaScript values were represented as a type tag and a value. The type tag for objects was 0. null was represented as the NULL pointer (0x00 in most platforms). Consequently, null had 0 as type tag, hence the "object" typeof return value.

- Const variables cannot be undefined.

- *Type coercion* is implicit type conversion by the Javascript engine, while *Type conversion* is, well explicit type conversion by the developer.