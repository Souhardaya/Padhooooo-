<!-- TOC -->

- [Understanding Promises in JavaScript](#understanding-promises-in-javascript)
  - [How to use a promise, in terms of an API ?](#how-to-use-a-promise-in-terms-of-an-api-)
  - [What are `.then` and `.catch` ?](#what-are-then-and-catch-)
  - [What are the three states of promise](#what-are-the-three-states-of-promise)
  - [Callbacks vs Promises](#callbacks-vs-promises)
  - [Promise chaining](#promise-chaining)
  - [Understanding `Promise.all` and `Promise.race`](#understanding-promiseall-and-promiserace)
  - [How do you prevent promises swallowing errors](#how-do-you-prevent-promises-swallowing-errors)
  - [How do you check an object is a promise or not](#how-do-you-check-an-object-is-a-promise-or-not)
  - [Promises VS Observable](#promises-vs-observable)
- [What is async/await ?](#what-is-asyncawait-)
  - [Difference between promise and async/await](#difference-between-promise-and-asyncawait)

<!-- /TOC -->

# Understanding Promises in JavaScript

A Promise is an object representing the eventual completion or failure of an asynchronous operation. Creating a promise:


```js
let myPromise = new Promise(function(myResolve, myReject) {
// "Producing Code" (May take some time)

  myResolve(); // when successful
  myReject();  // when error
});
```

A promise can be in one of three states:

- `pending`: The initial state of a promise. It represents an operation that has not yet completed, and the promise is neither fulfilled nor rejected.

- `fulfilled`: The state of a promise representing a successful operation. This is the state set when the promise's resolve() function is called.

- `rejected`: The state of a promise representing a failed operation. This is the state set when the promise's reject() function is called.

## How to use a promise, in terms of an API ?

```js

const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("foo");
  }, 300);
});

myPromise
  .then(handleFulfilledA, handleRejectedA)
  .then(handleFulfilledB, handleRejectedB)
  .then(handleFulfilledC, handleRejectedC);



```
## What are `.then` and `.catch` ?

The then() method returns a Promise. It takes up to two arguments: callback functions for the success and failure cases of the Promise.

Example:

```js

fetch('flowers.jpg')
.then(function(response) {
  if (!response.ok) {
    throw new Error('HTTP error, status = ' + response.status);
  }
  return response.blob();
})

```

The catch() method returns a Promise and deals with rejected cases only. It behaves the same as calling Promise.prototype.then(undefined, onRejected) (in fact, calling obj.catch(onRejected) internally calls obj.then(undefined, onRejected)).

Example:

```js

fetch('flowers.jpg')
.then(function(response) {
  if (!response.ok) {
    throw new Error('HTTP error, status = ' + response.status);
  }
  return response.blob();
})
.catch(function(error) {
  console.log(error);
})

```

## What are the three states of promise

Promises have three states:

1. **Pending:** This is an initial state of the Promise before an operation begins
2. **Fulfilled:** This state indicates that the specified operation was completed.
3. **Rejected:** This state indicates that the operation did not complete. In this case an error value will be thrown.


## Callbacks vs Promises

First of all let us take a quick look of how we use to code with callbackas and then we will see how promises are better than callbacks.

```js

const cart = ["shoes", "shirts", "socks"];

createOrder(cart, function(orderID){
    proceedToPayment(orderID);
});



const promise = createOrder(cart);
promise.then(function(orderID){
    proceedToPayment(orderID);
});
```
So first of all in the very first instance we were blindly trusting the `createOrder` function meaning that `createOrder` would be able to call the function whenever it wanted to

But in the second instance we are now attaching the callback function `proceedToPayment` to the `promise` object. Meaning we have alot of control with us. It will call the `proceedToPayment` function only when the `createOrder` function has been resolved.

And in case of errors we can use the `.catch` method to catch the error and handle it accordingly.

## Promise chaining

Promise chaining is used to execute multiple asynchronous operations sequentially. In other words, one after another. This is done by chaining `.then()` calls on the returned promise. The `.then()` method returns a new promise, different from the original one, which allows chaining.

```js

const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve(1);
  }, 2000);
});

promise
  .then((result) => {
    console.log(result); // 1
    return result * 2;
  })
  .then((result) => {
    console.log(result); // 2
    return result * 2;
  })
  .then((result) => {
    console.log(result); // 4
    return result * 2;
  });

```

In the example above, the promise is resolved after 2 seconds with the value `1`. The first `.then()` block is executed with the value `1` and returns `2`. The second `.then()` block is executed with the value `2` and returns `4`. The third `.then()` block is executed with the value `4` and returns `8`.

## Understanding `Promise.all` and `Promise.race`

These are two useful methods in JavaScript that deal with handling multiple promises. These methods are part of the Promises API, which provides a way to work with asynchronous operations in a more organized and convenient manner.

`Promise.all` is a method that takes an array of promises as input and returns a new promise that resolves when all the input promises have resolved or rejects when at least one of the input promises is rejected. The returned promise's resolution value will be an array containing the resolved values of each input promise, in the same order as the input array.

```js
const promise1 = Promise.resolve('Hello');
const promise2 = 42;
const promise3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'World');
});

Promise.all([promise1, promise2, promise3]).then(values => {
  console.log(values); // Output: ['Hello', 42, 'World']
}).catch(error => {
  console.error(error); // This will not be executed in this example
});

```

In the example above, `Promise.all` takes an array containing three elements: a resolved promise with the value `'Hello'`, a fulfilled promise with the value `42,` and a promise that resolves after a timeout with the value `'World'`. When all promises have resolved, the then block is executed with an array containing their resolved values.

If any of the promises passed to `Promise.all` rejects, the entire `Promise.all` call will reject immediately, and the catch block will be executed, skipping the then block.

`Promise.race`  is a method that takes an array of promises as input and returns a new promise that resolves or rejects as soon as one of the input promises resolves or rejects. The returned promise will have the same resolution value or rejection reason as the first promise that resolves or rejects among the input promises.

```js
const promise1 = new Promise((resolve) => setTimeout(resolve, 500, 'one'));
const promise2 = new Promise((resolve) => setTimeout(resolve, 100, 'two'));

Promise.race([promise1, promise2]).then((value) => {
  console.log(value); // Output: 'two' (because promise2 resolves first)
}).catch(error => {
  console.error(error); // This will not be executed in this example
});
```

In the example above, Promise.race takes an array containing two promises. Since `promise2` resolves faster than `promise1`, the then block is executed with the value `'two'`.


## How do you prevent promises swallowing errors

While using asynchronous code, JavaScript’s ES6 promises can make your life a lot easier without having callback pyramids and error handling on every second line. But Promises have some pitfalls and the biggest one is swallowing errors by default.

Let's say you expect to print an error to the console for all the below cases,

```javascript
Promise.resolve("promised value").then(function () {
  throw new Error("error");
});

Promise.reject("error value").catch(function () {
  throw new Error("error");
});

new Promise(function (resolve, reject) {
  throw new Error("error");
});
```

But there are many modern JavaScript environments that won't print any errors. You can fix this problem in different ways,

1. **Add catch block at the end of each chain:** You can add catch block to the end of each of your promise chains

```javascript
Promise.resolve("promised value")
  .then(function () {
    throw new Error("error");
  })
  .catch(function (error) {
    console.error(error.stack);
  });
```

But it is quite difficult to type for each promise chain and verbose too.

 2. **Add done method:** You can replace first solution's then and catch blocks with done method

```javascript
Promise.resolve("promised value").done(function () {
  throw new Error("error");
});
```

Let's say you want to fetch data using HTTP and later perform processing on the resulting data asynchronously. You can write `done` block as below,

```javascript
getDataFromHttp()
  .then(function (result) {
    return processDataAsync(result);
  })
  .done(function (processed) {
    displayData(processed);
  });
```

In future, if the processing library API changed to synchronous then you can remove `done` block as below,

```javascript
getDataFromHttp().then(function (result) {
  return displayData(processDataAsync(result));
});
```

and then you forgot to add `done` block to `then` block leads to silent errors.
    

## How do you check an object is a promise or not

If you don't know if a value is a promise or not, wrapping the value as `Promise.resolve(value)` which returns a promise

```javascript
function isPromise(object) {
  if (Promise && Promise.resolve) {
    return Promise.resolve(object) == object;
  } else {
    throw "Promise not supported in your environment";
  }
}

var i = 1;
var promise = new Promise(function (resolve, reject) {
  resolve();
});

console.log(isPromise(i)); // false
console.log(isPromise(promise)); // true
```

Another way is to check for `.then()` handler type

```javascript
function isPromise(value) {
  return Boolean(value && typeof value.then === "function");
}
var i = 1;
var promise = new Promise(function (resolve, reject) {
  resolve();
});

console.log(isPromise(i)); // false
console.log(isPromise(promise)); // true
```

## Promises VS Observable


| Promises                                                           | Observables                                                                              |
| ------------------------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| Emits only a single value at a time                                | Emits multiple values over a period of time(stream of values ranging from 0 to multiple) |
| Eager in nature; they are going to be called immediately           | Lazy in nature; they require subscription to be invoked                                  |
| Promise is always asynchronous even though it resolved immediately | Observable can be either synchronous or asynchronous                                     |
| Doesn't provide any operators                                      | Provides operators such as map, forEach, filter, reduce, retry, and retryWhen etc        |
| Cannot be canceled                                                 | Canceled by using unsubscribe() method                                                   |


# What is async/await ?

The async function declaration defines an asynchronous function, which returns an AsyncFunction object. An asynchronous function is a function which operates asynchronously via the event loop, using an implicit Promise to return its result. But the syntax and structure of your code using async functions is much more like using standard synchronous functions.

The await operator is used to wait for a Promise. It can only be used inside an async function.

The await expression causes async function execution to pause until a Promise is settled (that is, fulfilled or rejected), and to resume execution of the async function after fulfillment. When resumed, the value of the await expression is that of the fulfilled Promise.

## Difference between promise and async/await

The async/await syntax is built on top of promises. It cannot be used with plain callbacks or node callbacks. Async/await is actually just syntax sugar built on top of promises. It cannot be used with plain callbacks or node callbacks. Async/await is actually just syntax sugar built on top of promises, so it can only be used with functions that return promises. The async/await syntax allows you to write asynchronous code that reads similarly to synchronous code.

```js

async function myFunction() {
  try {
    const value = await promise;
    console.log(value);
  } catch (error) {
    console.log(error);
  }
}

```


