In JavaScript, hoisting allows you to use functions and variables before they're declared. It is important that you know about [[JavaScript#Execution Context|Execution Context]] to understand hoisting.

**Hoisting** in JavaScript is the behavior in which variable and function declarations are moved to the top of their containing scope (either function or global) during the creation phase of the execution context. This behavior allows variables and functions to be referenced before they are actually defined in the code, though the exact behavior differs for variable and function declarations.

So, "Hoisting" can be thought of as the process of **registering** variables and functions in memory during the **creation phase** of an execution context. This registration phase is what allows these variables and functions to be referenced before their line in the code.

#### Variable hoisting
As we know, there are three ways to declare a variable: `var`, `let`, and `const`. 

##### Variable hoisting with `var`
When the interpreter hoists a variable declared with `var`, it initializes its value to `undefined`. The first line of code below will output `undefined`:
```js
console.log(foo); // undefined

var foo = 'bar';

console.log(foo); // "bar"
```

As we defined earlier, hoisting moves declaration to top and assignment is done later during execution. We can achieve this same behavior manually by splitting the declaration and assignment into two steps:
```js
var foo;

console.log(foo); // undefined

foo = 'foo';

console.log(foo); // "foo"
```

Remember that the first `console.log(foo)` outputs `undefined` because `foo` is hoisted and given a default value (not because the variable is never declared). Using an undeclared variable will throw a `ReferenceError` instead:
```js
console.log(foo); // Uncaught ReferenceError: foo is not defined
```

##### Variable hoisting with `let` and `const`
Variables declared with `let` and `const` are **hoisted but not initialized with a default value**. 

Accessing a `let` or `const` variable before it's declared will result in a `ReferenceError`:
```js
console.log(foo); // Uncaught ReferenceError: Cannot access 'foo' before initialization

let foo = 'bar';  // Same behavior for variables declared with const
```

Notice that the `ReferenceError` is different from when we trying to access an undeclared variable(`Uncaught ReferenceError: foo is not defined`). The error message tells us the variable is initialized somewhere, this means the the interpreter still hoists `foo`but without a default value of `undefined`.


> [!warning] Outside Block Scope
> Trying to access the `const` or `let` variable outside its block scope will be same as if the variable did not exist. They are "hoisted" only to the top of their block scope. 

###### Temporal Dead Zone (TDZ)
The reason that we get a reference error when we try to access a `let` or `const` variable before its declaration is because of the temporal dead zone (TDZ).

The TDZ starts at the beginning of the variable's enclosing scope and ends when it is declared. Accessing the variable in this TDZ throws a `ReferenceError: Cannot access before initialization`.

Here's an example with an explicit block that shows the start and end of `foo`'s TDZ:
```js
{
    // Start of foo's TDZ
    let bar = "bar";
    console.log(bar); // "bar"

    console.log(foo); // ReferenceError because we're in the TDZ

    let foo = "foo"; // End of foo's TDZ
}
```

The TDZ is also present in default function parameters, which are evaluated left-to-right. In the following example, `bar` is in the TDZ until its default value is set:
```js
function foobar(foo = bar, bar = 'bar') {
  console.log(foo);
}
foobar(); // Uncaught ReferenceError: Cannot access 'bar' before initialization
```

But this code works because we can access `foo` TDZ ends before we try to access it:
```js
function foobar(foo = 'foo', bar = foo) {
  console.log(bar);
}
foobar(); // "foo"
```

#### Function Hoisting

Function declarations are hoisted, too. Function hoisting allows us to call a function before it is defined. For example, the following code runs successfully and outputs `"foo"`:
```js
foo(); // "foo"

function foo() {
    console.log('foo');
}
```

And since we know that the whole function code is hoisted( or registered) during creation phase, logging the function itself will output:
```js
console.log(foo);
// Ootput:
// ƒ foo() {
//     console.log("foo");
// }

function foo() {
    console.log("foo");
}
```

Note that only _function declarations_ are hoisted, not _function expressions_ or _arrow functions_. This makes sense because they are essentially variable declarations, and they behave accordingly.

###### Function Expressions
Function expressions are hoisted, but only the variable itself is hoisted, not the function assignment. This means trying to call the function before the line where it’s assigned will throw an error. The error will depend on whether you used `var` or `let`/`const`.

```js
console.log(expressionFunction()); 
// Throws: TypeError - expressionFunction is not a function

var expressionFunction = function() {
    return "I am a function expression.";
};
```
```js
console.log(expressionFunction()); 
// Throws: ReferenceError: Cannot access 'expressionFunction' before initialization

let expressionFunction = function () {
    return "I am a function expression.";
};
```

###### Arrow Functions
Arrow functions behave like function expressions in terms of hoisting. The variable is hoisted, but its assignment (the function definition) is not.

```js
console.log(arrowFunction()); 
// Throws: TypeError - arrowFunction is not a function

var arrowFunction = () => "I am an arrow function.";
```
```js
console.log(arrowFunction()); 
// Throws: ReferenceError: Cannot access 'arrowFunction' before initialization

const arrowFunction = () => "I am an arrow function.";
```