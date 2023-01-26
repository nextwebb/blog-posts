# Asynchronous programming in Node.js


Asynchronous I/O is a form of input/output processing that permits other processing to continue before the transmission has finished.

Asynchronous programming in JavaScript can only be achieved with functions being first-class citizens of the language. Thus functions can be passed around like any other variable to other functions. 
> 
Functions that can take other functions as arguments are called higher-order functions.

In Javascript, one efficient way of handling asynchronous or expensive operations is using promises. This is because in an asynchronous operation there is no guarantee of exactly when the operation will complete and the result it will return. 
A promise object thus helps us to cater for the intermediate state of the operation, whether it is successful(resolve), pending(ongoing), or fails(reject).

```
Promise.resolve() //success
Promise.reject() // fail
``` 


## Why Promises?
We established above that a Promise object represents the result of an asynchronous function. Promises can be used to avoid chaining of callbacks, referred to as the  `Callback hell`.

## let's get started by converting Node-styled callback to promise.

### Assumptions, let's set the tone for the discussion 😃
- You're familiar with javascript
- And you have some experience with the Node environment; if this is all checked ✅, we're set to learn about promises 😇.

It's easier to work with promises when working on async operations, but unfortunately, Nodejs has many of its APIs written with callbacks. (Promises were updates to the ECMAScript)

Here's an example using the file stream API. `fs.readFile`
```
const fs = require('fs') 

fs.readFile(filePath, options, callback)
``` 

Here, the callback function contains at least two arguments. The first argument must be an error object and the data returned.

```
fs.readFile('file-path', (err, data) => {
  if (err) {
     //throw new error
  } 
    //do something with data return
  
})
```
Below we convert a callback styled Node API to a promise, using the `util.promisify` API

```
const util = require('util');
const fs = require('fs');

const readFilePromise = util.promisify(fs.readFile)
readFilePromise('file-path').then((data) => {
    //do something with data return
}).catch((error) => {
    //throw new error
});

```
We can also use the `async/await` operation to resolve a promise. 


```
const readFilePromise = util.promisify(fs.readFile)

async function(){
  const data = await readFilePromise;
  console.log(data);
}


```
## Let's Create Promises 
In javascript, we can create asynchronous functions that return a promise 😊 . see the example below.

```
   const myPromise = new Promise((resolve, reject) = {
     setTimeout(() => 
       {
             console.log('resolving the promise ...');
             resolve('resolved!');
       }, 5000); // usually would be an asychronous operation

    reject('rejected!');
});

console.log(myPromise);

```

We can chain the Promises using `.then()` and `.catch()` higher-order functions. 
The `.then()` accepts a callback as an argument and invokes the resolved Promise. The `.catch()` accepts a callback and is executed when the Promise is rejected.

see the example below.

```
const resolvedCallback = (data) => console.log(data);
const rejectedCallback = (err) => console.log(err);

myPromise.then(resolvedCallback).catch(rejectedCallback);

```
## More examples handling Multiple independent promises
Unlike the examples we wrote above, we can return the state of a promise object without conditions. this might be useful when we need to `resolve` or `reject` multiple independent asynchronous operations. it's as simple as writing;

```
Promise.resolve();
Promise.reject();

```
We notice in the Node environment, the Promise object is globally available, interesting right! 😎

When dealing multiple independent promises, the methods `Promise.all()` and `Promise.race()` can be useful. Both methods accept an array of promises as an argument.

While using `Promise. all()`, the `.then` method is only invoked after the last promise is `resolved`.

```
const promiseArr = [
  Promise.resolve("data1")
  Promise.resolve("data2")
  Promise.resolve("data3")
]
```

```
Promise.all(promiseArr).then(results =>
  console.log(results)
)

// [ data1, data2, data3 ]
```
While using `Promise.race()`, the `.then` method is invoked only when the first promise is `resolved`.


```
Promise.race(promiseArr).then(results =>
  console.log(results)
)

// data1

```
One thing to note as well is that the least expensive function will always resolve first. 
 This enables us to run multiple asynchronous operations and act upon the one that finishes first.


### Wrap-up ✍️

- A promise represents the state of execution of a function
- A promise has 3 states; resolve, reject and pending. 
- Using async and await is a sugar coating wrapped around promises and it makes working with promises easy and readable
- Await ensures it resolves the promise being executed and encourages return error early.
- Promises and async/await accomplish the same thing. 
   - They make retrieving and handling expensive functions/asynchronous data easier.
   - They eliminate the need for callback hell/nested functions
   - Simplify error handling.
   - Reduce extraneous code
   - Make waiting for multiple concurrent calls to return and adding additional code in between calls easier.



Thanks for the audience and I hope you found this article helpful getting started with promises in Javascript 🤗. Feel free to reach out to me on  [Github](https://github.com/nextwebb), [Twitter](https://twitter.com/i_am_nextwebb) and [LinkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/).
Do drop a like, comment, and share 😌.

  [Find out more about Graceful asynchronous programming with Promises](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Promises) 