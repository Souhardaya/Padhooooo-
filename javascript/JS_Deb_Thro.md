<!-- TOC -->

- [What is Debouncing ?](#what-is-debouncing-)
  - [Understanding details](#understanding-details)
  - [Workflow](#workflow)
- [Throttling](#throttling)
  - [Understanding details](#understanding-details-1)
  - [Workflow](#workflow-1)

<!-- /TOC -->


# What is Debouncing ? 

Debouncing is a programming practice used to ensure that time-consuming tasks do not fire so often, that it stalls the performance of the web page. In other words, it limits the rate at which a function gets invoked. Suppose we have an input box and we are constantly listening to the input event on that input box. Now, if we want to perform some action on every input event, then we can do that. But, if we want to perform some action after the user has stopped typing, then we can use debouncing.

```js
const userInput = document.getElementById("userinput");

// this is your main function that will be doing tasks, api calls and what not
const filterElements = (targetValue) => {
    console.log("Hi, I was called 👉 ", targetValue);

    userArr.filter((curElem) => {
        curElem.innerText.toLowerCase().includes(targetValue.toLowerCase()) ?
            curElem.classList.remove('hide') :
            curElem.classList.add('hide')
    })
};

// this is the core logic
const debounce = (callback, delay) => {
    let timer;
    return (...args) => {
        clearTimeout(timer);
        timer = setTimeout(() => {
            callback(...args);
        }, delay);
    };
};

const debouncedFilter = debounce(filterElements, 1000);

// Add event listener with the debounced function
userInput.addEventListener("input", () => {
    const inputText = userInput.value.trim();
    debouncedFilter(inputText);
});

```


## Understanding details

- In our very first function we have `filterElements`, this can be something else too like calling an API or something else. This is the function that will be called after the user has stopped typing. </br></br>
- Next we have `debounce` function, this is the core logic of our debouncing. This function takes two arguments, first is the `function that we want to call after the user has stopped typing` and the second argument is the `delay after which we want to call that function`. I will talk more about this later. </br></br>
- Finally we have `debouncedFilter`, we are now calling the `debounce(filterElements, 1000)` - remeber that the `debounce` function returns us a very important function, so we are storing that function in `debouncedFilter` variable. </br></br>
- Last but not least we are adding an event listener to our input box, and we are calling the `debouncedFilter` function inside it along with the texts that the user is typing. </br></br>

## Workflow

- When the user starts typing, the `debouncedFilter` function is called with the text that the user is typing. </br></br>


- Remember how i told you earlier that the `debounce` function returns us a very important function, so we are storing that function in `debouncedFilter` variable? Now it's time to call that function as we are writing `debouncedFilter(inputText)` </br></br>

- Once we call the `debouncedFilter` function, it will first clear the timer that we have set earlier to keep a fresh timer. By clearing the previous timeout, you ensure that the `callback` function will only run after the most recent delay and not be affected by any previous calls.  </br></br>

- The `debounced` function will only execute the callback function after the specified delay (1000ms) when there are no further calls to the `debounced` function within that delay period.   </br></br>


# Throttling

Throttling is a technique in which, no matter how many times the user fires the event, the attached function will be executed only once in a given time interval. Suppose we have an input box and we are constantly listening to the input event on that input box. Now, if we want to perform some action on every input event, then we can do that. But, if we want to perform some action after a certain interval of time, then we can use throttling.

```js
const userButton = document.getElementById("userbutton");

const throttleHandler = () => {
    console.log("Hi, I was clicked");
};


const throttle = (callback, delay) => {
    let isWaiting = false;
    return () => {
        if (!isWaiting) {
            callback();
            isWaiting = true;
            setTimeout(() => {
                isWaiting = false;
            }, delay);
        }
    };
};

// Throttled click event handler
const throttledClickHandler = throttle(throttleHandler, 1000);

// Adding event listener with the throttled click handler
userButton.addEventListener("click", throttledClickHandler);
```

## Understanding details

-  We first have our `throttleHandler` function this is the function that will be called at every interval. We are just console logging you can do anything here. </br></br>
-  Next we have our `throttle` function whis again returns us a very important function. This function takes two arguments, first is the `function that we want to call` and the second argument is the `delay after which we want to call that function`. I will talk more about this later. </br></br>
- Next we have `throttledClickHandler` function, we are now calling the `throttle(throttleHandler, 1000)` - remeber that the `throttle` function returns us a very important function, so we are storing that function in `throttledClickHandler` variable. </br></br>

- Last but not least we are adding an event listener to our button, and we are calling the `throttledClickHandler` function inside it. </br></br>


## Workflow

- When the user clicks on the button, the `throttledClickHandler` function is called. </br></br>

- Remember how i told you earlier that the `throttle` function returns us a very important function, so we are storing that function in `throttledClickHandler` variable? Now it's time to call that function as we are writing `userButton.addEventListener("click", throttledClickHandler)` </br></br>

- Inside of that imporant function we have alot of things such as `isWaiting` variable, this variable is used to check if the user is waiting or not. By default it  will be false, so as soon as it is false we are gonna call the `throttleHandler` that was passed to us.  </br></br>

- Next we simply set the `isWaiting` variable to true, so that the next time the user clicks on the button, the `throttleHandler` function will not be called. </br></br>

- We also have a `setTimeout` function, this is used to set the `isWaiting` variable to false after a certain interval of time so that the function is called again. </br></br>