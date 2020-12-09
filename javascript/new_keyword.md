# `new` keyword

An Object can be created using the `new` keyword.
Let's see the steps it performs while creating an object.

Let's see how new keyword creates an object using following example.

        function MyFunc() {
            var myVar = 1;
            this.x = 100;
        }

        MyFunc.prototype.y = 200;

        var obj1 = new MyFunc();
        obj1.x; // 100
        obj1.y; // 200

Let's understand what happens when you create an object (instance) of MyFunc() using new keyword.

First of all, new keyword creates an empty object - { }.

Second, it set's invisible 'prototype' property (or attribute) of this empty object to myFunc's prototype property. As you can see in the above example, we have assigned new property 'y' using MyFunc.prototype.y. So, new empty object will also have same prototype property as MyFunc which includes y property.

In third step, it binds all the properties and function declared with this keyword to new empty object. Here, MyFunc includes only one property x which is declared with this keyword. So new empty object will now include x property. MyFunc also includes myVar variable which does not declared with this keyword. So myVar will not be included in new object.

In the fourth and last step, it will return this newly created object. MyFunc does not include return statement but compiler will implicitly insert 'return this' at the end.

So thus, object of MyFunc will be returned using new keyword.

The following figure illustrates the above process.

![Object Creation Process](/images/new-keyword.png "Object Creation Process")

- The new keyword ignores return statement that returns primitive value.

    function MyFunc() {
        this.x = 100;
        
        return 200;
    }

    var obj = new MyFunc();
    alert(obj.x); // 100

- If function returns non-primitive value (custom object) then new keyword does not perform above 4 tasks.

        function MyFunc() {
            this.x = 100;
                    
            return { a: 123 };
        }

        var obj1 = new MyFunc();

        alert(obj1.x); // undefined

Thus, new keyword builds an object of a function in JavaScript.