# `this` Keyword

The `this` keyword is one of the most widely used and yet confusing keyword in JavaScript.

The following four rules applies to `this` in order to know which object is referred by this keyword.

- Global Scope
- Object's Method
- call() or apply() method
- bind() method

## Global Scope

If a function which includes `this` keyword, is called from the global scope then this will point to the window object.

        <script>
        var myVar = 100;

        function WhoIsThis() {
            var myVar = 200;

            alert("myVar = " + myVar); // 200
            alert("this.myVar = " + this.myVar); // 100
        }

        WhoIsThis(); // inferred as window.WhoIsThis()
        </script>

The global scope means in the context of window object. We can optionally call it like `window.WhoIsThis()`. So in the above example, this keyword in `WhoIsThis()` function will refer to window object. So, `this.myVar` will return 100.

- In the strict mode, value of `this` will be *undefined* in the global scope.
- `this` points to global window object even if it is used in an inner function.

## `this` Inside Object's Method
When you create an object of a function using new keyword then this will point to that particular object.

    var myVar = 100;

    function WhoIsThis() {
        this.myVar = 200;
    }
    var obj1 = new WhoIsThis();

    var obj2 = new WhoIsThis();
    obj2.myVar = 300;

    alert(obj1.myVar); // 200 
    alert(obj2.myVar); // 300 


## call() and apply()
In JavaScript, a function can be invoked using () operator as well as call() and apply() method as shown below.

        function WhoIsThis() {
            alert('Hi');
        }

        WhoIsThis();
        WhoIsThis.call();
        WhoIsThis.apply();

In the above example, WhoIsThis(), WhoIsThis.call() and WhoIsThis.apply() executes a function in the same way.

The main purpose of call() and apply() is to set the context of `this` inside a function irrespective whether that function is being called in the global scope or as object's method.

You can pass an object as a first parameter in call() and apply() to which the `this` inside a calling function should point to.

The following example demonstrates the call() & apply().

        var myVar = 100;

        function WhoIsThis() {

            alert(this.myVar);
        }

        var obj1 = { myVar : 200 , test: WhoIsThis };

        var obj2 = { myVar : 300 , test: WhoIsThis };

        WhoIsThis(); // 'this' will point to window object

        WhoIsThis.call(obj1); // 'this' will point to obj1

        WhoIsThis.apply(obj2); // 'this' will point to obj2

        obj1.whoIsThis.call(window); // 'this' will point to window object

        WhoIsThis.apply(obj2); // 'this' will point to obj2

As you can see in the above example, when the function WhoIsThis is called using () operator (like WhoIsThis()) then `this` inside a function follows the rule- refers to window object. However, when the WhoIsThis is called using `call()` and `apply()` method then `this` refers to an object which is passed as a first parameter irrespective of how the function is being called.

Therefore, `this` will point to obj1 when a function got called as WhoIsThis.call(obj1). In the same way,  `this` will point to obj2 when a function got called like `WhoIsThis.apply(obj2)`

## bind()

The bind() method was introduced since ECMAScript 5. It can be used to set the context of 'this' to a specified object when a function is invoked.

The bind() method is usually helpful in setting up the context of this for a callback function. Consider the following example.

        var myVar = 100;
            
        function SomeFunction(callback)
        {
            var myVar = 200;

            callback();
        };
            
        var obj = {
                    myVar: 300,
                    WhoIsThis : function() {
                        alert("'this' points to " + this + ", myVar = " + this.myVar);
                    }
            };
            
        SomeFunction(obj.WhoIsThis); 
        SomeFunction(obj.WhoIsThis.bind(obj));

In the above example, when you pass `obj.WhoIsThis` as a parameter to the SomeFunction() then `this` points to global window object insted of obj, because `obj.WhoIsThis()` will be executed as a global function by JavaScript engine. You can solve this problem by explicitly setting this value using bind() method. Thus, `SomeFunction(obj.WhoIsThis.bind(obj))` will set this to obj by specifying `obj.WhoIsThis.bind(obj)`.

## Precedence
So these 4 rules applies to this keyword in order to determine which object this refers to. The following is precedence of order from highest to lowest.

1. bind()
2. call() and apply()
3. Object method
4. Global scope

So, first check whether a function is being called as callback function using bind()? If not then check whether a function is being called using call() or apply() with parmeter? If not then check whether a function is being called as an object function? Otherise check whether a function is being called in the global scope without dot notation or using window object.

Thus, use these simple rules in order to know which object the 'this' refers to inside any function.
