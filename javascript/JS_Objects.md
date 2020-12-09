- JavaScript object is a standalone entity that holds multiple values in terms of properties and methods.
- Object property stores a literal value and method represents function.
- An object can be created using object literal or object constructor syntax.
- **Object literal:**

        var person = { 
            firstName: "James", 
            lastName: "Bond", 
            age: 25, 
            getFullName: function () { 
                return this.firstName + ' ' + this.lastName 
                } 
        };
- **Object constructor:**

        var person = new Object();
                            
        person.firstName = "James";
        person["lastName"] = "Bond"; 
        person.age = 25;
        person.getFullName = function () {
                return this.firstName + ' ' + this.lastName;
            };

- Object properties and methods can be accessed using dot notation or [ ] bracket.
- An object is passed by reference from one function to another.
- An object can include another object as a property.
- Any javascript function using which object is created is called **constructor function.**
- Use **Object.keys()** method to retrieve all the properties name for the specified object as a string array.

        function Student(){
        this.title = "Mr.";
        this.name = "Steve";
        this.gender = "Male";
        this.sayHi = function(){    
            alert('Hi');
        }
        }
        var student1 = new Student();

        Object.keys(student1); // ["title", "name", "gender", "sayHi"]

        //enumerate properties of student1
        for(var prop in student1){
        console.log(prop); // title name gender sayHi
        }

## Property Descriptor

In JavaScript, each property of an object has property descriptor which describes the nature of a property. Property descriptor for a particular object's property can be retrieved using `Object.getOwnPropertyDescriptor()` method.

`Object.getOwnPropertyDescriptor(object, 'property name')`

The getOwnPropertyDescriptor method returns a property descriptor for a property that directly defined in the specified object but not inherited from object's prototytpe.

the property descriptor includes the following 4 important attributes.

| Attributes      | Description |
| ----------- | ----------- |
| value      | Contains an actual value of a property.       |
| writable   | Indicates that whether a property is writable or read-only. If true than value can be changed and if false then value cannot be changed and will throw an exception in strict mode        |
| enumerable | Indicates whether a property would show up during the enumeration using for-in loop or Object.keys() method.        |
| configurable | Indicates whether a property descriptor for the specified property can be changed or not. If true then any of this 4 attribute of a property can be changed using Object.defineProperty() method.        |



## Object.defineProperty()
The Object.defineProperty() method defines a new property on the specified object or modifies an existing property or property descriptor.

Syntax: 

`Object.defineProperty(object, 'property name', descriptor)`

For example, if we set the `name` property above to `writable: false`, then if you try to change the value of `name` property then it would throw an exception in strict mode. In non-strict mode, it won't throw an exception but it also won't change a value of name property either.

[Link for further reading.](https://www.tutorialsteacher.com/javascript/javascript-object-in-depth)