# JavaScript Classes

ECMAScript 2015, also known as ES6, introduced JavaScript Classes.

JavaScript Classes are templates for JavaScript Objects.

# JavaScript Class Syntax

Use the keyword class to create a class. 
Always add a method named constructor():

        class ClassName {
        constructor() { ... }
        }


**A JavaScript class is not an object.**

**It is a template for JavaScript objects.**

# Using a Class
When you have a class, you can use the class to create objects:

    let myCar1 = new Car("Ford", 2014);
    let myCar2 = new Car("Audi", 2019);

The example above uses the Car class to create two Car objects.

The constructor method is called automatically when a new object is created.