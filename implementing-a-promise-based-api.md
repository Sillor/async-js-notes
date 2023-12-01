# How to implement a promise-based API
Generally, when you implement a promise-based API, you'll be wrapping an asynchronous operation, which might use events, or plain callbacks, or a message-passing model. You'll arrange for a `Promise` object to handle the success or failure of that operation properly.

## Implementing an `alarm()` API
In this example we'll implement a promise-based alarm API, called `alarm()`. It will take as arguments the name of the person to wake up and a delay in milliseconds to wait before waking the person up. After the delay, the function will send a "Wake up!" message, including the name of the person we need to wake up.
 ### Wrapping `setTimeout()`
 `alarm()` API will use `setTimeout()` that will call the function after the specific time

HTML:
 ```html
 <button id="set-alarm">Set alarm</button>
<div id="output"></div>
 ```
 JS:
 ```js
 const output = document.querySelector("#output");
const button = document.querySelector("#set-alarm");

function setAlarm() {
  setTimeout(() => {
    output.textContent = "Wake up!";
  }, 1000);
}

button.addEventListener("click", setAlarm);
 ```

### The `Promise()` constructor
Our `alarm()` function will return a Promise that is fulfilled when the timer expires. It will pass a "Wake up!" message into the `then()` handler, and will reject the promise if the caller supplies a negative delay value.
```js
function alarm(person, delay) {
  return new Promise((resolve, reject) => {
    if (delay < 0) {
      throw new Error("Alarm delay must not be negative");
    }
    setTimeout(() => {
      resolve(`Wake up, ${person}!`);
    }, delay);
  });
}
```

## Using the `alarm()` API
Here is an example code with usage case:
```js
const name = document.querySelector("#name");
const delay = document.querySelector("#delay");
const button = document.querySelector("#set-alarm");
const output = document.querySelector("#output");

function alarm(person, delay) {
  return new Promise((resolve, reject) => {
    if (delay < 0) {
      throw new Error("Alarm delay must not be negative");
    }
    setTimeout(() => {
      resolve(`Wake up, ${person}!`);
    }, delay);
  });
}

button.addEventListener("click", () => {
  alarm(name.value, delay.value)
    .then((message) => (output.textContent = message))
    .catch((error) => (output.textContent = `Couldn't set alarm: ${error}`));
});
```

## Using async and await with the `alarm()` API
It is also possible to use promise chaining, `Promise.all()`, and async/await:

```js
const name = document.querySelector("#name");
const delay = document.querySelector("#delay");
const button = document.querySelector("#set-alarm");
const output = document.querySelector("#output");

function alarm(person, delay) {
  return new Promise((resolve, reject) => {
    if (delay < 0) {
      throw new Error("Alarm delay must not be negative");
    }
    setTimeout(() => {
      resolve(`Wake up, ${person}!`);
    }, delay);
  });
}

button.addEventListener("click", async () => {
  try {
    const message = await alarm(name.value, delay.value);
    output.textContent = message;
  } catch (error) {
    output.textContent = `Couldn't set alarm: ${error}`;
  }
});
```