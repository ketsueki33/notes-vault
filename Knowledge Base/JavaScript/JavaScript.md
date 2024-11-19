![[_assets/JS_logo.png|150]]

***Map of Content***
- [[#Introduction]]
- [[Variables & Data Types]]
	- [[Variables & Data Types#Difference between `let` and `var`|Difference between let and var]]
- [[Operators]]
- [[Control Structures]]
- [[Loops]]
- [[Functions]]
	- [[Functions#Difference between Regular Functions and Arrow Functions|Difference between Regular Functions and Arrow Functions]]
- [[../JavaScript/Arrays|Arrays]]
- [[Objects]]
- [['this' keyword]]
- [[ES6+ Features]]
- [[DOM Manipulation]]
- [[DOM Event Handling]]
- [[Async JavaScript]]
- [[Error Handling]]
- [[Modules]]
- [[Classes (OOP)]]
- [[Closures]]
- [[Event Loop]]
- [[Hoisting]]

*Extras:*
- [[Interview Tidbits]]

todo:- currying (in closures), prototypal inheritance (objects)

### Introduction
JavaScript is a scripting or programming language that allows you to implement complex features on web pages. It is the third layer of the layer cake of standard web technologies (after [[../HTML|HTML]] and [[../CSS/CSS|CSS]]).

JavaScript is a **synchronous, single-threaded, interpreted** language.

Every browser has its own JavaScript engine. Google Chrome has the V8 engine, Mozilla Firefox has SpiderMonkey, and so on. They all are used for the same goal, because the browsers cannot directly understand JavaScript code.

#### Execution Context
When the JavaScript engine scans a script file, it makes an environment called the **Execution Context** that handles the entire transformation and execution of the code.

There are two types of execution contexts: **global** and **function**. The global execution context is created when a JavaScript script first starts to run, and it represents the global scope in JavaScript. A function execution context is created whenever a function is called, representing the function's local scope.

##### Phases of the JavaScript Execution Context

There are two phases of JavaScript execution context:

1. **Creation phase**: In this phase, the JavaScript engine creates the execution context and sets up the script's environment. It determines the values of variables and functions and sets up the scope chain for the execution context.
2. **Execution phase**: In this phase, the JavaScript engine executes the code in the execution context. It processes any statements or expressions in the script and evaluates any function calls.
##### Components of the JavaScript Execution Context

So, everything in JavaScript happens inside an Execution Context. It is *divided into two components*. 
1. **Memory Component** : This is where all variables and functions are stored as key-value pairs. This component is also knows as *Variable Environment*.
2. **Code Component** : This is where each line of source code is executed from top to bottom. This component is also knows as *Thread of Execution*.

![[_assets/execution_context_1.png|450]]

It is important to remember that these phases and components are applicable to both global and functional execution contexts.


##### Example Run
Let's take this code:
```js
var n = 5;

function square(n) {
  var ans = n * n;
  return ans;
}

var square1 = square(n);
var square2 = square(8);  

console.log(square1)
console.log(square2
```

**Creation Phase**
At the very beginning, the JavaScript engine executes the entire source code, creates a global execution context, and then does the following things:

1. Creates a global object that is **window** in the browser and **global** in NodeJs.
2. Sets up a memory for storing variables and functions.
3. Stores the variables with values as undefined and function references.

Variables are assigned a default value of `undefined` ( in case of `var`). (see [[Hoisting]] to understand other cases)

Functions are assigned the whole code of that function.

Here's a diagram to show how the execution context looks after creation phase:

![[_assets/execution_context_2.png|450]]


**Execution Phase**
Now, in this phase, it starts going through the entire code line by line from top to bottom. As soon as it encounters **n = 5**, it assigns the value 5 to 'n' in memory. Until now, the value of 'n' was undefined by default.

Then we get to the 'square' function. As the function has been allocated in memory, it directly jumps into the line **var square1 = square(n);**. square() will be invoked and JavaScript once again will create a new function execution context.

![[_assets/execution_context_3.png|450]]

Once the calculation is done, it assigns the value of square in the 'ans' variable that was undefined before. The function will return the value, and the function execution context will be destroyed.

The returned value from square() will be assigned on square1. This happens for square2 also. Once the entire code execution is done completely, the global context will look like this and it will be destroyed also.

![[_assets/execution_context_4.png|450]]

#### Call Stack
To keep the track of all the contexts, including global and functional, the JavaScript engine uses a **call stack**. A call stack is also known as an 'Execution Context Stack', 'Runtime Stack', or 'Machine Stack'.

**Call Stack maintains the order of execution of execution contexts.**

When the engine first starts executing the script, it creates a global execution context and pushes it on the stack. So, the *global execution context will always be at the bottom of the call stack*.

Whenever a function is invoked, similarly, the JS engine creates a function stack context for the function and pushes it to the top of the call stack and starts executing it. When execution of the current function is complete, then the JavaScript engine will automatically remove the context from the call stack and the control goes back to its parent.

Too understand how the Call Stack works with asynchronous operations, see [[Event Loop]].

**Stack Overflow**
The call stack has its own fixed size depending on the system or browser. If the number of contexts exceeds the limit, then a stack overflow error will occur. This happens with a recursive function that has no base condition.

#### Scope Chain

The **scope chain** in JavaScript is the mechanism that allows variables and functions to be accessed in nested scopes. When a variable or function is referenced in a piece of code, JavaScript will try to resolve that reference by looking up the **scope chain** until it either finds the variable or reaches the global scope.

##### How the Scope Chain Works

1. **Lexical Environment**:
    
    - JavaScript determines scope based on the structure of the code (lexical scope), which means that the scope of a variable is defined by where it is declared in the code.
    - Each function and block creates a new **lexical environment** (scope) that contains references to its own variables and has a reference to the outer environment.
2. **Chain of Scopes**:
    
    - When a variable is referenced, JavaScript starts looking for it in the current scope.
    - If it’s not found there, it moves up to the next scope in the chain (the outer lexical environment).
    - This continues until JavaScript either finds the variable or reaches the **global scope**.
    - If it reaches the global scope and the variable is still not found, a `ReferenceError` is thrown.

##### Example to Illustrate the Scope Chain
```js
let globalVar = 'global';

function outerFunction() {
    let outerVar = 'outer';

    function innerFunction() {
        let innerVar = 'inner';
        
        console.log(innerVar);   // Logs 'inner' (found in innerFunction scope)
        console.log(outerVar);   // Logs 'outer' (found in outerFunction scope)
        console.log(globalVar);  // Logs 'global' (found in global scope)
    }
    
    innerFunction();
}

outerFunction();
```

- **Global Scope**:
    
    - Contains `globalVar` and the function `outerFunction`.
- **outerFunction Scope**:
    
    - Contains `outerVar` and the function `innerFunction`, plus a reference to the global scope.
- **innerFunction Scope**:
    
    - Contains `innerVar` and references to the scopes of `outerFunction` and the global scope.
    - When `innerFunction` is called, it can access `innerVar` (defined in its own scope), `outerVar` (from `outerFunction`'s scope), and `globalVar` (from the global scope), following the scope chain.

