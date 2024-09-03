To understand how JavaScript executes asynchronous operations see [[Event Loop]]
#### Callback Hell
Callback hell, also known as the "pyramid of doom," refers to the situation in JavaScript where multiple nested callbacks are used to handle asynchronous operations. This nesting creates a code structure that is difficult to read, maintain, and debug. As each callback function introduces another level of indentation, the code becomes increasingly nested and more complex.

**Example:**
Imagine you need to perform a series of asynchronous operations, such as reading a file, processing its contents, and then saving the results to a new file. Using traditional callbacks, the code might look like this:
```js
fs.readFile('file1.txt', 'utf8', (err, data1) => {
  if (err) {
    console.error(err);
    return;
  }
  processFile(data1, (err, data2) => {
    if (err) {
      console.error(err);
      return;
    }
    fs.writeFile('file2.txt', data2, (err) => {
      if (err) {
        console.error(err);
        return;
      }
      console.log('File written successfully');
    });
  });
});
```

**Issues with Callback Hell:** Readability, Scalability, Error Handling, Maintainability

To avoid this callback hell , we have *promises* and *async functions*.

#### Promises
Promises represent a value that may be available now, or in the future, or never. A Promise is an object that represents the eventual completion (or failure) of an asynchronous operation and its resulting value. (eg. getting data from a Movie API)

**A Promise can be in one of the three states:**
1. Pending: The initial state, neither fulfilled nor rejected.
2. Fulfilled: The operation completed successfully, and the promise has a value.
3. Rejected: The operation failed, and the promise has a reason for the failure.

##### Creating A Promise
A Promise is created using the `Promise` constructor, which takes a function called the executor. This function has two parameters: `resolve` and `reject`. `resolve` is called when the operation is successful, and `reject` is called when the operation fails.
```js
const myPromise = new Promise((resolve, reject) => {
  const success = true; // Simulate success or failure
  
  if (success) {
    resolve('Operation succeeded');
  } else {
    reject('Operation failed');
  }
});
```

##### Handling Promises
Promises are handled using the `.then()`, `.catch()`, and `.finally()` methods.
- **`.then(onFulfilled, onRejected)`**: Adds fulfillment and rejection handlers.
- **`.catch(onRejected)`**: Adds a rejection handler.
- **`.finally(onFinally)`**: Adds a handler to be called regardless of the promise’s outcome.

##### Chaining Promises
Promises can be chained to perform a series of asynchronous operations in sequence. We do this by returning the next promise within the previous callback.
```js
function fetchData(x) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve("Data " + x);
        }, 1500);
    });
}

fetchData(1)
    .then((result1) => {
        console.log(result1); // Logs: Data 1
        return fetchData(2);
    })
    .then((result2) => {
        console.log(result2); // Logs: Data 2
        return fetchData(3);
    })
    .then((result3) => {
        console.log(result3); // Logs: Data 3
        console.log("All data fetched");
    })
    .catch((error) => {
        console.error("Error fetching data:", error);
    });
```

##### Concurrent Promises
When dealing with multiple asynchronous operations that can run concurrently (in parallel), using concurrent promises becomes essential. This is where `Promise.all` and `Promise.race` methods come into play.

###### Promise.all
`Promise.all` takes an array (or any iterable) of promises and returns a single promise that resolves when all of the promises in the array have resolved, or rejects when any one of the promises in the array rejects.
```js
const promise1 = new Promise((resolve) => setTimeout(() => resolve("one"), 1000));
const promise2 = new Promise((resolve) => setTimeout(() => resolve("two"), 2000));
const promise3 = new Promise((resolve) => setTimeout(() => resolve("three"), 3000));

Promise.all([promise1, promise2, promise3])
    .then((values) => {
        console.log(values); // ["one", "two", "three"]
    })
    .catch((error) => {
        console.error(error);
    });
```

**Behavior:**
- Resolves: When all promises in the iterable have resolved. The resulting promise will resolve to an array of the results.
- Rejects: When any one of the promises in the iterable rejects. The resulting promise will reject with the reason of the first promise that rejects.

###### Promise.race
`Promise.race` takes an array (or any iterable) of promises and returns a single promise that resolves or rejects as soon as one of the promises in the array resolves or rejects.
```js
const promise1 = new Promise((resolve) => setTimeout(() => resolve("one"), 1000));
const promise2 = new Promise((resolve) => setTimeout(() => resolve("two"), 2000));
const promise3 = new Promise((resolve) => setTimeout(() => resolve("max verstappen"), 300));

Promise.race([promise1, promise2, promise3])
    .then((value) => {
        console.log(value); // "max verstappen"
    })
    .catch((error) => {
        console.error(error);
    });
```

**Behavior:**
- Resolves: When the first promise in the iterable resolves. The resulting promise will resolve to the value of the first resolved promise.
- Rejects: When the first promise in the iterable rejects. The resulting promise will reject with the reason of the first rejected promise.

#### Async/Await
Async functions and the `await` keyword provide a way to work with asynchronous code in a more readable and straightforward manner compared to using Promises directly.
##### Async Functions
An async function is a function declared with the `async` keyword. It always returns a Promise, even if the function does not explicitly return a promise. If the function returns a value, the promise will be resolved with that value. If the function throws an error, the promise will be rejected with that error.

```js
async function fetchData() {
  return "Data fetched!";
}

fetchData().then(data => console.log(data)); // Output: Data fetched!
```

##### await Keyword
The `await` keyword can only be used inside an async function. **It pauses the execution of the async function and lets JavaScript continue executing the next set of functions in the [[Event Loop#Call Stack|Call Stack]] until the promise resolves and returns its result**. If the promise is rejected, it throws the rejected value.
```js
async function myFunction() {
  let result = await somePromise;
}
```

##### Error Handling inside Async Functions
Errors in async functions can be handled using try...catch blocks, making it easier to handle errors compared to traditional promise chains. 
```js
async function asyncFunctionWithError() {
  try {
    let response = await fetch('invalid-url');
    let data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Error:', error); // Handle the error
  }
}

asyncFunctionWithError();
```

#### Fetch API
The Fetch API provides a JavaScript interface for making HTTP requests and processing the responses.

With the Fetch API, you make a request by calling `fetch()`, which is available as a global function in both `window` and `worker` contexts. You pass it a `Request` object or a string containing the URL to fetch, along with an optional argument to configure the request.

**without Async/Await**
```js
function fetchData() {
    fetch("https://jsonplaceholder.typicode.com/posts/1")
        .then((response) => response.json())
        .then((data) => {
            console.log(data);
        })
        .catch((error) => console.error("error:", error));
}

fetchData();
```

**with Async/Await**
```js
async function fetchData() {
    try {
        const reponse = await fetch("https://jsonplaceholder.typicode.com/posts/2");
        const data = await reponse.json();
        console.log(data);
    } catch (error) {
        console.error("error:", error);
    }
}

fetchData();
```