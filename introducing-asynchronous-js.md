# Introducing asynchronous JavaScript
Asynchronous programming is a technique that allows the program to start a long-running task and still be responsive to other events (continue code).

Some examples of async. tasks:
- Making HTTP requests using `fetch()`
- Accessing a user's camera or microphone using `getUserMedia()`
- Asking a user to select files using `showOpenFilePicker()`

## Synchronous programming
An example of synchronous code:
```js
function makeGreeting(name) {
  return `Hello, my name is ${name}!`;
}

const name = "Miriam";
const greeting = makeGreeting(name);
console.log(greeting);
// "Hello, my name is Miriam!"
```

Each line in the code above is executed in order, one line at a time, even if there are functions. This makes the code synchronous.

The problem appears when there is a long, time-consuming function that does not allow other code to execute while it's going.

## Event handlers
Event handlers represent a form of asynchronous programming: you provide a function (the event handler) that will be called, not right away, but whenever the event happens

## Callbacks
An event handler is a particular type of callback. A callback is just a function that's passed into another function, with the expectation that the callback will be called at the appropriate time.

However, callback-based code can get hard to understand when the callback itself has to call functions that accept a callback. This is a common situation if you need to perform some operation that breaks down into a series of asynchronous functions.

Here is a simple code:
```js
function doStep1(init) {
  return init + 1;
}

function doStep2(init) {
  return init + 2;
}

function doStep3(init) {
  return init + 3;
}

function doOperation() {
  let result = 0;
  result = doStep1(result);
  result = doStep2(result);
  result = doStep3(result);
  console.log(`result: ${result}`);
}

doOperation();
```

Here is the same code implemented using callbacks:
```js
function doStep1(init, callback) {
  const result = init + 1;
  callback(result);
}

function doStep2(init, callback) {
  const result = init + 2;
  callback(result);
}

function doStep3(init, callback) {
  const result = init + 3;
  callback(result);
}

function doOperation() {
  doStep1(0, (result1) => {
    doStep2(result1, (result2) => {
      doStep3(result2, (result3) => {
        console.log(`result: ${result3}`);
      });
    });
  });
}

doOperation();
```
This code above create a so-called "callback hell" because it is much harder to read and debug. For this reason, JavaScript's foundation of asynchronous programming is based on `Promises`.