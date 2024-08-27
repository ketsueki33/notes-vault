#### throw Statement
The `throw` statement is used to create custom errors. When a `throw` statement is encountered, the execution of the current function will stop, and control will be passed to the first `catch` block in the call stack. If no `catch` block exists among the calling functions, the program will terminate.

**syntax:**
```js
throw expression;
```
*expression:* The expression to throw. This can be a string, number, boolean, object, or an instance of the Error class.

##### throwing a string
```js
try {
  throw "An error occurred";
} catch (e) {
  console.log(e); // "An error occurred"
}
```

##### throwing a number
```js
try {
  throw 404;
} catch (e) {
  console.log(e); // 404
}
```

##### throwing an object
```js
try {
  throw { message: "Something went wrong", code: 500 };
} catch (e) {
  console.log(e.message); // "Something went wrong"
  console.log(e.code);    // 500
}
```

#### Error object
The `Error` object in JavaScript represents an error that occurs during the execution of a program. It provides a standardized way to handle and communicate errors. The `Error` object can be used to create custom error messages and to catch exceptions using `try...catch` statements.
```js
try {
  throw new Error("An unexpected error occurred");
} catch (e) {
  console.log(e.name);    // "Error"
  console.log(e.message); // "An unexpected error occurred"
}
```

##### Built-in Error Types
JavaScript has several built-in error types that extend the `Error` object. These include:
1. `SyntaxError`: An error that occurs when there is a syntax mistake in the code.
2. `ReferenceError`: An error that occurs when referencing a variable that is not declared.
3. `TypeError`: An error that occurs when a value is not of the expected type.
4. `RangeError`: An error that occurs when a numeric variable or parameter is outside its valid range.
5. `EvalError`: An error related to the global eval() function. This error is not commonly used in modern JavaScript.
6. `URIError`: An error that occurs when there is an issue with URI handling functions such as decodeURI() or encodeURI().

##### Custom Error Objects
You can create your own custom error types by extending the `Error` object. This allows you to create more specific error handling in your applications.
```js
class CustomError extends Error {
  constructor(message) {
    super(message);
    this.name = "CustomError";
    this.color = "rainbow";
  }
}

try {
  throw new CustomError("This is a custom error");
} catch (e) {
  console.log(e.name);    // "CustomError"
  console.log(e.message); // "This is a custom error"
}
```

#### try...catch...finally 
In JavaScript, the `try...catch...finally` statement is used to handle exceptions and perform cleanup actions. It provides a way to handle errors gracefully and ensure that certain code executes regardless of whether an error occurred or not.
```js
//Syntax:
try {
  // Code that may throw an error
} catch (error) {
  // Code to handle the error
} finally {
  // Code that will always run, regardless of an error
}
```

 **try Block**: The code that might throw an error is placed inside the `try` block. If no error occurs, the `catch` block is skipped.

**catch Block**: If an error occurs in the `try` block, the control is transferred to the `catch` block, where you can handle the error. The `catch` block can access the error object, which contains details about the error.

 **finally Block**: The `finally` block contains code that will execute regardless of whether an error occurred in the `try` block or not. This is useful for cleanup actions, such as closing files or releasing resources.

*Example:*
```js
function divide(a, b) {
  try {
    if (b === 0) {
      throw new Error("Division by zero");
    }
    let result = a / b;
    console.log(result);
  } catch (error) {
    console.log("An error occurred: " + error.message);
  } finally {
    console.log("Division operation complete.");
  }
}

divide(10, 2);  // 5, "Division operation complete."
divide(10, 0);  // "An error occurred: Division by zero", "Division operation complete."
```

You can use a `try...finally` block without a `catch` block if you only need to ensure cleanup actions are performed.

#### Error Handling in Promises
[[Async JavaScript#Promises|Promises]] represent a value that may be available now, or in the future, or never. In asynchronous programming with JavaScript, error handling is important due to the potential for errors in asynchronous operations.

##### `.catch()` for error handling
see [[Async JavaScript#Handling Promises|Handling Promises]]

##### `try...catch` in async functions
see [[Async JavaScript#Error Handling inside Async Functions|Error Handling inside Async Functions]]



