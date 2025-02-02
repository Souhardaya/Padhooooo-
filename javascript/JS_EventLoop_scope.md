
<!-- TOC -->

- [Understand the call stack](#understand-the-call-stack)
  - [So how are code executed ?](#so-how-are-code-executed-)
  - [What is global execution context?](#what-is-global-execution-context)
  - [What is function execution context?](#what-is-function-execution-context)
- [The super powers](#the-super-powers)
  - [So how do we access these super powers ?](#so-how-do-we-access-these-super-powers-)
- [Callback queue and event loop](#callback-queue-and-event-loop)
  - [Event Loop](#event-loop)
    - [What are different event loops](#what-are-different-event-loops)
  - [Behind the scenes of `setTimeout()`](#behind-the-scenes-of-settimeout)
  - [`setTimeout()` breaks trust](#settimeout-breaks-trust)
  - [How do you implement zero timeout in modern browsers](#how-do-you-implement-zero-timeout-in-modern-browsers)
  - [`setTimeout(0)` is not 0](#settimeout0-is-not-0)
  - [Behind the scenes of DOM api](#behind-the-scenes-of-dom-api)
- [MicroTask Queue](#microtask-queue)
  - [What is microtask ?](#what-is-microtask-)
  - [Behind the scenes of `fetch()`](#behind-the-scenes-of-fetch)
  - [What is the purpose of queueMicrotask](#what-is-the-purpose-of-queuemicrotask)
- [Why do we even need callback queue ?](#why-do-we-even-need-callback-queue-)
- [Starvation of callback queue](#starvation-of-callback-queue)
- [The scope chain, scope \& lexical environment 🔥](#the-scope-chain-scope--lexical-environment-)
  - [Deep diving](#deep-diving)
  - [Scope chaining, and scopes](#scope-chaining-and-scopes)

<!-- /TOC -->

# Understand the call stack 

So javascript is a synchronous single threaded language so it has a call stack and it can do one thing at a time. And call stack doesn't have a timer so whatever you are gonna be putting inside of there will be immediately executed.


## So how are code executed ?

So this call stack is actually present inside the javascript engine,  and whenever any javascript code is to be run a global execution context is created and that is pushed inside of the call stack first. 

 - Then suppose we have a function so anything for that function will be pushed into the call stack and that function will be executed and popped out from the call stack

 - And once that is done then we pop out the global execution context from the call stack.


## What is global execution context?

The global execution context is the default or first execution context that is created by the JavaScript engine before any code is executed(i.e, when the file first loads in the browser). All the global code that is not inside a function or object will be executed inside this global execution context. Since JS engine is single threaded there will be only one global environment and there will be only one global execution context.

## What is function execution context?

Whenever a function is invoked, the JavaScript engine creates a different type of Execution Context known as a Function Execution Context (FEC) within the Global Execution Context (GEC) to evaluate and execute the code within that function.


# The super powers

We want to do a lot of things with javascript we need a timer we need something to access location something to access the different kind of storage servers across the world and so on.  but with our call stack all of those functionalities are limited and so we take the help of our browsers superpower.  

So in such cases we take the help of web api's to do our task.

- setTimeout()
- localStorage
- DOM Apis
- fetch()
- console.log()
- location

All of these are a part of the browsers web apis even the, document. Stuff we write.
So if we write window.setTimeout(), or anything - we access those super powers basically. 

We don’t need to write window as it’s in global scope/object

## So how do we access these super powers ?

So we access these super powers by writing `window.setTimeout()` or `window.localStorage` or `window.document` and so on. But since we are in the global scope we don't need to write window as it is in the global scope. So we can just write `setTimeout()` or `localStorage` or `document` and so on.

As soon as we would write `console.log()` we are actually using the `console.log()` method of the `window` object. 

# Callback queue and event loop

The task of the callback queue is to store all the callback functions that are ready to be executed.

## Event Loop

The event loop is a continuous loop that runs in the background of the JavaScript runtime environment. The main task of it is to first of all check the microtask queue , then the callback queue and when the callstack is empty it will push stuffs from the queue into the callstack.

### What are different event loops

In JavaScript, there are multiple event loops that can be used depending on the context of your application. The most common event loops are:

1. The Browser Event Loop
2. The Node.js Event Loop

- Browser Event Loop: The Browser Event Loop is used in client-side JavaScript applications and is responsible for handling events that occur within the browser environment, such as user interactions (clicks, keypresses, etc.), HTTP requests, and other asynchronous actions.

- The Node.js Event Loop is used in server-side JavaScript applications and is responsible for handling events that occur within the Node.js runtime environment, such as file I/O, network I/O, and other asynchronous actions.

## Behind the scenes of `setTimeout()`

Take a look at the code:

```javascript

console.log('1');
setTimeout(() => {
    console.log('2');
}, 5000);
console.log('3');

```

- So when we run this code, the first thing that happens is that the global execution context is created and pushed into the call stack.

- Then the first `console.log('1')` is executed and popped out from the call stack.

- Then the `setTimeout()` is executed and popped out from the call stack.

- Now the `setTimeout()` is a web api and it has a timer of 5 seconds so it will be pushed into the **callback queue after 5 seconds**.

- Then the `console.log('3')` is executed and popped out from the call stack.

- Now the call stack is empty and the event loop will check if the call stack is empty and if it is empty then it will check if there is anything in the callback queue and if there is anything in the callback queue then it will push it into the call stack.

- So after 5 seconds the `setTimeout()` will be pushed into the call stack and executed and popped out from the call stack.

- So the event loop will keep on checking if the call stack is empty and if it is empty then it will check if there is anything in the callback queue and if there is anything in the callback queue then it will push it into the call stack.


## `setTimeout()` breaks trust

So as we have heard the timeout method is actually breaking the trust but how is it able to do so ?

It's very simple as we already studied all of these kind of things like the timeout fetch and everything first goes into the callback queue and after that it comes into the call stack so for example take into consideration of huge amount of code

People there is a million lines of code that is being executed in the call stack,  and suppose we say that it takes about 10 seconds for that code to execute and only after that the call stack will be empty and time out will be pushed inside the call stack.

Now here's the trick inside of our timeout we set a timer for five seconds but it will be executed after 10 seconds because the call stack was super busy in executing other things so that is the reason why it seems that the set timeout is breaking our trust.

## How do you implement zero timeout in modern browsers

You can't use setTimeout(fn, 0) to execute the code immediately due to minimum delay of greater than 0ms. But you can use window.postMessage() to achieve this behavior.

## `setTimeout(0)` is not 0

So we have seen that the `setTimeout()` is breaking our trust but what about the `setTimeout(0)` ?

So the thing is that the `setTimeout(0)` is not 0 it is actually 4 milliseconds. So the thing is that the `setTimeout()` is not actually a part of the javascript engine it is a part of the web api's and the web api's are not a part of the javascript engine they are a part of the browser. So the browser has a minimum time of 4 milliseconds for the `setTimeout()`.




## Behind the scenes of DOM api

Consider this following code:

```javascript

console.log('1');

document.getElementById('btn').addEventListener('click', () => {
    console.log('2');
});

console.log('3');

```

- So when we run this code, the first thing that happens is that the global execution context is created and pushed into the call stack.

- Then the first `console.log('1')` is executed and popped out from the call stack.

- Then we call the DOM api it will find the element with the id `btn` and add an event listener to it and then it sits there.

- Also, the `console.log('3')` is executed and popped out from the call stack.

- Now as soon as we click the button the event listener will be pushed into the callback queue and then the event loop will push it into the call stack and execute it and pop it out from the call stack.

# MicroTask Queue

So we have seen that the callback queue is used to store all the callback functions that are ready to be executed. But there is another queue called the `Micro Task queue` which is used to store all the promises and mutation observers.

Introduced with ECMAScript 6, this queue is used to store microtasks. 

## What is microtask ?

Microtasks are tasks with higher priority than regular tasks in the task queue. Promises (created using the Promise constructor or async/await syntax) and certain APIs like process.nextTick (in Node.js) are executed as microtasks. Microtasks are processed before the next rendering or UI update, making them suitable for critical updates that need to happen before the browser repaints.

They are kind of blocking in nature. i.e, The main thread will be blocked until the microtask queue is empty.
The main sources of microtasks are Promise.resolve, Promise.reject, MutationObservers, IntersectionObservers etc

## Behind the scenes of `fetch()`

```javascript

console.log("Start");

setTimeout(function cbT(){
    console.log("Callback cbT");
}, 5000);


fetch('https://api.github.com/users')
    .then(function cbF(){
        console.log("Callback cbF");
    });

console.log("End");

```

Everything works the same, except the fact that anything that come through the promises OR mutation observers will end up in the `Micro Task queue.` which is of higher priority than the callback queue.

So here, `cbF()` will be pushed into the `Micro Task queue` and then the event loop will push it into the call stack and execute it and pop it out from the call stack and after that `cbT()` will be pushed into the `callback queue` and then the event loop will push it into the call stack and execute it and pop it out from the call stack.

## What is the purpose of queueMicrotask
    
The `queueMicrotask` function is used to schedule a microtask, which is a function that will be executed asynchronously in the microtask queue. The purpose of `queueMicrotask` is to ensure that a function is executed after the current task has finished, but before the browser performs any rendering or handles user events.

Example:

```javascript
console.log('Start'); //1

queueMicrotask(() => {
  console.log('Inside microtask'); // 3
});

console.log('End'); //2
```
By using queueMicrotask, you can ensure that certain tasks or callbacks are executed at the earliest opportunity during the JavaScript event loop, making it useful for performing work that needs to be done asynchronously but with higher priority than regular `setTimeout` or `setInterval` callbacks.

# Why do we even need callback queue ?

See the thing is if the user was to click the button multiple times then the event listener would be pushed into the callback queue multiple times and the event loop would push it into the call stack multiple times and execute it multiple times. That is the reason why we maintain a callback queue.



# Starvation of callback queue

If our micro task queue is full then the callback queue will starve and the callback queue will not be able to push anything into the call stack as the event loop will keep on pushing the micro task queue into the call stack.

# The scope chain, scope & lexical environment 🔥

So whenever we are gonna call a function as we already know first of all a global execution context is made ready and then it assigns some memory to a function. 

After that different execution stacks are then created and all execution stacks will be made for all the functions that we currently have and after that those functions will be executed one after the another.  and now whenever an execution stack is created and lexical environment is also created.


The word `Lexical` means `in hierarchy` or `in order` 

`Lexical Environment` is created whenever an execution context is created, it is basically the local memory plus the parents lexical environment. 


## Deep diving


```js

function a() {
    var b = 10;
    c();
    function c() {
        console.log(b);
    }
}

a();

```

The output will be 10.

- Here `c()` is lexically inside of `a()`, so `c()'s` lexical environment is whatever we have inside of `a()` plus a reference to the lexical environment of the global execution context which is `a()`'s parent.


- So when the execution stack is created for the function `c()` then the lexical environment will be created for the function `c()` and the lexical environment will contain the local memory of the function `c()` and a reference to the lexical environment of the function `a()` which is it's parent.

- Same for `a()` it will have a reference to the lexical environment of the global execution context which is it's parent.

- That is the reason why we can acess `b` even if it is not present in the lexical environment of the function `c()`, because it is present in the lexical environment of the function `a()` which is it's parent.




## Scope chaining, and scopes

Imagine that we donot have `var b = 10` anywhere in the code. So first we will look inside `c()` , then in it's parent `a()` and then in it's parent which is the global execution context. And if we donot find it anywhere then we will get an error, meaning that `b` is out of scope.

Scope basically means "where" you can acess a variable or a function in your code.

And the mechanism as to how we were searching for `b` in different lexical environments is called `scope chaining`. If the scope chain is exhausted and we still donot find the variable then we will get an error.