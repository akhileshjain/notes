# JavaScript Modules

JavaScript programs started off pretty small — most of its usage in the early days was to do isolated scripting tasks, providing a bit of interactivity to your web pages where needed, so large scripts were generally not needed. Fast forward a few years and we now have complete applications being run in browsers with a lot of JavaScript, as well as JavaScript being used in other contexts (Node.js, for example).

It has therefore made sense in recent years to start thinking about providing mechanisms for splitting JavaScript programs up into separate modules that can be imported when needed. Node.js has had this ability for a long time, and there are a number of JavaScript libraries and frameworks that enable module usage (for example, other **CommonJS** and **AMD-based module systems** like **RequireJS**, and more recently **Webpack** and **Babel**).

The good news is that modern browsers have started to support module functionality natively, and this is what this article is all about. 

Use of native JavaScript modules is dependent on the `import` and `export` statements which are supported by most of modern browsers.

## Exporting Module Features

The first thing you do to get access to module features is export them. This is done using the `export` statement.

The easiest way to use it is to place it in front of any items you want exported out of the module, for example -

        export const name = 'square';

        export function draw(ctx, length, x, y, color) {
        ctx.fillStyle = color;
        ctx.fillRect(x, y, length, length);

        return {
            length: length,
            x: x,
            y: y,
            color: color
        };
        }

You can export functions, var, let, const, and  even classes. They need to be top-level items; you can't use export inside a function, for example.

A more convenient way of exporting all the items you want to export is to use a single export statement at the end of your module file, followed by a comma-separated list of the features you want to export wrapped in curly braces. For example:

`export { name, draw, reportArea, reportPerimeter };`

## Importing Features into your script

Once you've exported some features out of your module, you need to import them into your script to be able to use them. The simplest way to do this is as follows:

`import { name, draw, reportArea, reportPerimeter } from './modules/square.js';`

You use the import statement, followed by a comma-separated list of the features you want to import wrapped in curly braces, followed by the keyword from, followed by the path to the module file .

## Default Exports vs Named Exports

The functionality we've exported so far has been comprised of named exports — each item (be it a function, const, etc.) has been referred to by its name upon export, and that name has been used to refer to it on import as well.

There is also a type of export called the default export — this is designed to make it easy to have a default function provided by a module, and also helps JavaScript modules to interoperate with existing CommonJS and AMD module systems.

Example

`export default nameSquare`

Note the lack of curly braces.

We could instead prepend export default onto the function and define it as an anonymous function, like this:

    export default function(ctx) {
        ...
    }

Over in our `main.js` file, we import the default function using this line:

import `randomSquare` from `'./modules/square.js'`;

Again, note the lack of curly braces. This is because there is only one default export allowed per module, and we know that randomSquare is it. The above line is basically shorthand for:

import `{default as randomSquare}` from `'./modules/square.js';`

## Avoiding Naming Conflicts

what happens if we try to add a module that deals with drawing another shape, like a circle or triangle? These shapes would probably have associated functions like draw(), reportArea(), etc. too; if we tried to import different functions of the same name into the same top-level module file, we'd end up with conflicts and errors.

Fortunately there are a number of ways to get around this. We'll look at these in the following sections.

## Renaming imports and exports

Inside your import and export statement's curly braces, you can use the keyword as along with a new feature name, to change the identifying name you will use for a feature inside the top-level module.

### Example
    // inside module.js
    export {
    function1 as newFunctionName,
    function2 as anotherNewFunctionName
    };

    // inside main.js
    import { newFunctionName, anotherNewFunctionName } from './modules/module.js';

It arguably makes more sense to leave your module code alone, and make the changes in the imports. This especially makes sense when you are importing from third party modules that you don't have any control over.

## Creating a Module Object

The above method works OK, but it's a little messy and longwinded. An even better solution is to import each module's features inside a module object. For example 

`import * as Module from './modules/module.js';`
This grabs all the exports available inside module.js, and makes them available as members of an object Module, effectively giving it its own namespace. So for example:

`Module.function1()`

`Module.function2()`
etc...

So you can now write the code just the same as before (as long as you include the object names where needed), and the imports are much neater.

## Modules and Classes

 You can also `export` and `import` classes; this is another option for avoiding conflicts in your code, and is especially useful if you've already got your module code written in an object-oriented style.

You can see an example of our shape drawing module rewritten with ES classes in our classes directory. As an example, the `square.js` file now contains all its functionality in a single class.

`class Square {`

`constructor(ctx, id, length, x, y, color) {`

`...`

`}`

`draw() {`

`...`

`}`

`}`

which we then export:

`export {Square};`

Over in `main.js`, we import it like this:

`import { Square } from './modules/square.js';`

And then use the class to draw our square:

`let square1 = new Square(myCanvas.ctx,`

 `myCanvas.listId, 50, 50, 100, 'blue');`


`square1.draw();`

`square1.reportArea();`

`square1.reportPerimeter();`

---

## Dynamic Module Loading

The newest part of the JavaScript modules functionality to be available in browsers is dynamic module loading. This allows you to dynamically load modules only when they are needed, rather than having to load everything up front. This has some obvious performance advantages; let's read on and see how it works.

This new functionality allows you to call `import()` as a function, passing it the path to the module as a parameter. It returns a `Promise`, which fulfills with a module object  giving you access to that object's exports, e.g.

    import('./modules/myModule.js')
    .then((module) => {
        // Do something with the module.
    });


### Another Example

    squareBtn.addEventListener('click', () => {
    import('./modules/square.js').then((Module) => {
        let square1 = new Module.Square(myCanvas.ctx, myCanvas.listId, 50, 50, 100, 'blue');
        square1.draw();
        square1.reportArea();
        square1.reportPerimeter();
    })
    });

Note that, because the promise fulfillment returns a module object, the class is then made a subfeature of the object, hence we now need to access the constructor with Module. prepended to it, e.g. `Module.Square( ... )`.