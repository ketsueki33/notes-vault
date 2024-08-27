JavaScript is single threaded. It can only process one thing at a time. Then how does it handle multiple tasks at the same time? 

**JavaScript Engine has a mechanism  to handle incoming tasks which includes the Event Loop, Call Stack and Task Queue**

#### Call Stack
The call stack keeps track of what's being executed along with their order. When a function is invoked or called, it gets added to the top of the Call Stack. It starts executing and once the function 
is done executing, it gets removed from the stack. 

So, if the function takes 5 seconds to execute, it will block the Call Stack for 5 seconds. Here is where Asynchronous functions come in. They are sent to some other entity ( like  the Web API of a  browser) and it gets removed from the call stack immediately. It is now the responsibility of this entity to finish the execution of that function. The Call Stack is not blocked during this and continues executing the next functions. 

Once the other entity finishes the execution of the async function, it sends the callback to the Task Queue.

#### Task Queue
The Task Queue stores callbacks or jobs that come back from async operations like timeouts, network requests, user events etc. 

These callbacks need to go back to the Call Stack to finish execution.
#### Event Loop
Event Loop is responsible for moving things from the Task Queue to the Call Stack.

Event loop constantly observes the Call Stack to check if it's empty. If it is empty it will look inside the Task Queue for things that are waiting for execution. If there's anything in there, the Event Loop will move it from the Task Queue to the Call Stack for execution.

> [!important]  Micro-tasks vs Macro-tasks 
> There are two separate Task Queues for micro-tasks and macro-tasks.
> 
> The micro-task queue contains the callbacks of operations that are considered more urgent or important, such as **promises** and mutation observers APIs.
> 
> The macro-task queue contains the callbacks of operations that are less urgent such as timers, I/O events,DOM Manipulation and user interface events.
> 
> The event loop always gives **higher priority to the micro-task queue**, and will process all the callbacks in the microtask queue before moving on to the macrotask queue.

##### Examples:

**Example- SetTimeout with 0 second timeout**
```js
console.log(1);

setTimeout(()=>{
	console.log(2);
}, 0)

console.log(3);
//Output:
// 1
// 3
// 2
```
3 will get printed before 2 as the Event Loop will wait for the synchronous operations to finish before putting the callback to print 2 in the Call Stack.  


**Example- fetching data from backend using async function**
```js
async function fetchData() {
    console.log("1. Starting fetch...");
    let response = await fetch('https://api.example.com/data'); // Call fetch, start async op
    console.log("3. Data fetched!"); // Only after fetch resolves
    let data = await response.json(); // Another await, starts async JSON parsing
    console.log("4. Data parsed!"); // After JSON parsing resolves
}

console.log("0. Before fetch");
fetchData();
console.log("2. After fetch call");

```

1. **Async Function and Call Stack**
When you call an `async` function, it is added to the **call stack** like any other function. However, as soon as it hits an `await` keyword while fetching data, the function execution is paused at that point.

2. **Fetch Request**
The `await` keyword is usually followed by a promise (e.g., a `fetch` request). When the `fetch` function is called:

- The JavaScript engine initiates the asynchronous operation to fetch data from the backend.
- This asynchronous operation is handed off to the browser's Web APIs (for example, the networking layer) and is not blocking the main thread.
- The `fetch` request is now running in the background.

3. **Returning Control**
Once the `await` keyword is encountered, the `async` function effectively returns control back to the caller, and the `async` function is removed from the **call stack**.

4. **Call Stack Continues**
With the `async` function paused, the call stack can continue to execute any remaining synchronous code.

5. **Fetch Completes**
When the backend responds, the `fetch` request completes. The result (a `Promise`) is resolved and the callback attached to this promise is sent to the **task queue** (sometimes referred to as the "microtask queue" for promises).

6. **Event Loop and Task Queue**
The **event loop** constantly checks the **call stack**. If the call stack is empty (i.e., no other synchronous code is running), the event loop picks the next task from the **task queue** (in this case, the resolved fetch promise) and pushes its callback onto the call stack.

7. **Resuming the Async Function**
Once the callback is pushed onto the call stack, it will resolve the `await` and resume the `async` function from where it left off. The data fetched from the backend is now available to the `async` function.

8. **Completion**
The rest of the `async` function executes, potentially encountering other `await` expressions, which follow the same process. Once the `async` function completes, it is removed from the call stack.

