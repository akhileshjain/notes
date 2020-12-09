## call() vs apply() vs bind()

Use `.bind()` when you want that function to later be called with a certain context, useful in events. Use .`call()` or `.apply()` when you want to invoke the function immediately, and modify the context.

Call/apply call the function immediately, whereas bind returns a function that, when later executed, will have the correct context set for calling the original function. This way you can maintain context in async callbacks and events.

`apply` is similar to `call` except that it takes an array-like object instead of listing the arguments out one at a time.

`bind()` is great for **Function Currying.** Currying is the process of taking a function with multiple arguments and returning a series of functions that take one argument and eventually resolve to a value. It is a great technique that tries to minimize the number of changes to a program’s state (known as side effects) by using immutable data and pure (no side effects) functions.


|  |Time of function execution|Time of this binding|
|---|---|---|
|`function` object f| future | future|
|`function` call f()| now | now |
|`f.call()`, `f.apply()`| now | now|
|`f.bind()`| future | now|
