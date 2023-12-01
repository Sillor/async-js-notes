# How to use promises
Promises are the foundation of asynchronous programming in modern JavaScript. A promise is an object returned by an asynchronous function, which represents the current state of the operation. They allow to track whether the operation was successful or threw an error.

## Using the fetch() API
Allows to send requests to a remote server.
Here is an example code:

```js
const fetchPromise = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);

console.log(fetchPromise);

fetchPromise.then((response) => {
  console.log(`Received response: ${response.status}`);
});

console.log("Started request…");
```

## Chaining promises
With the `fetch()` API, once you get a `Response` object, you need to call another function to get the response data. In this case, we want to get the response data as JSON, so we would call the `json()` method of the Response object. It turns out that `json()` is also asynchronous. So this is a case where we have to call two successive asynchronous functions.

Basic Example of using chaining promises using `then()`:
```js
const fetchPromise = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);

fetchPromise
  .then((response) => response.json())
  .then((data) => {
    console.log(data[0].name);
  });
```

## Catching errors
The `fetch()` API also supports error handling. If the server returns an error, we can catch it using `catch()` method when the asynchronous operation fails.

```js
const fetchPromise = fetch(
  "bad-scheme://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);

fetchPromise
  .then((response) => {
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    return response.json();
  })
  .then((data) => {
    console.log(data[0].name);
  })
  .catch((error) => {
    console.error(`Could not get products: ${error}`);
  });
  ```

## Promise terminology
A promise has three states:
- **pending**: the promise has been created, and the asynchronous function it's associated with has not succeeded or failed yet. This is the state your promise is in when it's returned from a call to `fetch()`, and the request is still being made.
- **fulfilled**: the asynchronous function has succeeded. When a promise is fulfilled, its `then()` handler is called.
- **rejected**: the asynchronous function has failed. When a promise is rejected, its `catch()` handler is called.

## Combining multiple promises
In a case when you need to fullfill multiple promises that don't depend on each other, you can use `Promise.all()` method. It takes an array of promises and returns a single promise.

```js
const fetchPromise1 = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);
const fetchPromise2 = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/not-found",
);
const fetchPromise3 = fetch(
  "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json",
);

Promise.all([fetchPromise1, fetchPromise2, fetchPromise3])
  .then((responses) => {
    for (const response of responses) {
      console.log(`${response.url}: ${response.status}`);
    }
  })
  .catch((error) => {
    console.error(`Failed to fetch: ${error}`);
  });
  ```

## Async and await
The `async` keyword gives you a simpler way to work with asynchronous promise-based code. Adding `async` at the start of a function makes it an async function:
```js
async function myFunction() {
  // This is an async function
}
```

Inside an async function, you can use the await keyword before a call to a function that returns a promise. This makes the code wait at that point until the promise is settled, at which point the fulfilled value of the promise is treated as a return value, or the rejected value is thrown.

```js
async function fetchProducts() {
  try {
    // after this line, our function will wait for the `fetch()` call to be settled
    // the `fetch()` call will either return a Response or throw an error
    const response = await fetch(
      "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
    );
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    // after this line, our function will wait for the `response.json()` call to be settled
    // the `response.json()` call will either return the parsed JSON object or throw an error
    const data = await response.json();
    console.log(data[0].name);
  } catch (error) {
    console.error(`Could not get products: ${error}`);
  }
}

fetchProducts();
```

Also, note that you can only use `await` inside an `async` function, unless your code is in a JavaScript module.