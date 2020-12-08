A callback is a function that is to be executed after another function has finished executing — hence the name **call back**.

*More complexly put* - In JavaScript, functions are objects. Because of this, functions can take functions as arguments, and can be returned by other functions. Functions that do this are called **higher-order functions**. Any function that is passed as an argument is called a **callback function**.

## Synchronous Callback Function
If your code executes sequentially from top to bottom, it is synchronous. The isOddNumber() function is an example of a synchronous callback function.

In the following example, the arrow function is a callback used in a synchronous function.

The sort() method completes first before the console.log() executes:

`let numbers = [1, 2, 4, 7, 3, 5, 6];`

` numbers.sort((a, b) => a - b);`

` console.log(numbers); // [ 1, 2, 3, 4, 5, 6, 7 ]`

## Asynchronous Callback Function
Asynchronicity means that if JavaScript has to wait for an operation to complete, it will execute the rest of the code while waiting.

Note that JavaScript is a single-threaded programming language. It carries asynchronous operations via the callback queue and event loop.

Suppose that you need to develop a script that downloads a picture from a remote server and process it after the download completes:

    function download(url) {
        // Asynchronous code, may take 2-3 sec.
    }

    function process(picture) {
        //...
    }

    download(url);
    process(picture);

However, downloading a picture from a remote server takes time depending on the network speed and the size of the picture.

Therefore the type of output you may get might be something like this -

Processing https://javascripttutorial.net/foo.jg
Downloading https://javascripttutorial.net/foo.jg ...

This is not what you expected because the `process()` function executes before the `download()` function. The correct sequence should be:

- Download the picture, wait for it to complete.

- Process the picture.

To fix the issue above, you can pass the process() function to the download() function and execute the process() function inside the download() function once the download completes, like this:

    function download(url, callback) {
    
     setTimeout(() => {
            // script to download the picture here
            console.log(`Downloading ${url} ...`);
            
            // process the picture once it is completed
            callback(url);
        }, 3000);
    }

    function process(picture) {
        console.log(`Processing ${picture}`);
    }

    let url = 'https://wwww.javascripttutorial.net/pic.jpg';
    download(url, process);

Output in this case will then be -

    Downloading https://www.javascripttutorial.net/pic.jpg ...
    Processing https://www.javascripttutorial.net/pic.jpg

Now, it works as expected.

In this example, the process() is a callback passed into an asynchronous function.

When you use callbacks to continue code execution after asynchronous operations, these callbacks are called asynchronous callbacks.

By using asynchronous callbacks, you can register an action in advance without blocking the entire operation.

To make the code cleaner, you can define the process() function as an anonymous function:

    function download(url, callback) {
    setTimeout(() => {
        // script to download the picture here
        console.log(`Downloading ${url} ...`);
        // process the picture once it is completed
        callback(url);

        }, 3000);
    }
    let url = 'https://www.javascripttutorial.net/pic.jpg';
    download(url, function(picture) {
        console.log(`Processing ${picture}`);
    });

## Nesting Callbacks and the Pyramid of Doom

Now, suppose you want to download multiple files sequentially. A typical approach is to call the `download()` function inside the callback function

However, this callback strategy does not scale well when the complexity grows significantly.

Nesting many asynchronous functions inside callbacks is known as the **pyramid of doom** or the **callback hell**:

    asyncFunction(function(){
        asyncFunction(function(){
            asyncFunction(function(){
                asyncFunction(function(){
                    asyncFunction(function(){
                        ....
                    });
                });
            });
        });
    });

To avoid the pyramid of doom, you use promises or async/await functions.    