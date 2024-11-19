#### Variable Declaration
##### Using `var`
```js
var a = 5;
console.log(a);
// 5
```

##### Using `let`
```js
let b = "hello";
console.log(b);
// hello
```

#### Constant Declaration
```js
const c = false;
console.log(c);
// false
```

##### const with objects and arrays
A const object's properties can be modified.
A const array in JavaScript can be modified.

*For const arrays:*
- You cannot reassign the array variable to a new array.

*For const objects:* 
- While you cannot reassign the object variable, you can modify its properties.
```js
// Const array
const arr = [1, 2, 3];

arr = [4, 5, 6];  // Error: Assignment to a constant variable
arr.push(4);      // This works!
arr[3] = 4;       // This works!

arr[0] = 10;      // This works! You can modify existing elements

// Const object
const obj = { a: 1, b: 2 };

obj = { c: 3 };   // Error: Assignment to a constant variable
obj.a = 10;       // This works! You can modify existing properties
obj.c = 3;        // This works! You can add new properties
```

#### Difference between `let` and `var` 

| Feature                        | `let`                                                                                      | `var`                                                                                            |
| ------------------------------ | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------ |
| **Scope**                      | Variables declared by `let` are **only available inside the block** where they’re defined. | Variables declared by `var` are **available throughout the function** in which they’re declared. |
| **Hoisting**                   | Hoisted to top of block, but in Temporal Dead Zone (TDZ) until initialized                 | Hoisted to top of function or global scope, can be accessed before declaration (undefined)       |
| **Redeclaration**              | Not allowed within the same scope                                                          | Allowed within the same scope                                                                    |
| **Initialization Requirement** | Must be initialized after declaration, otherwise causes ReferenceError                     | Can be accessed without initialization, defaults to `undefined`                                  |
| **Global Object Property**     | Does not attach to the global object when declared in global scope                         | Attaches to the global object when declared in global scope                                      |
| **Temporal Dead Zone (TDZ)**   | Exists in the TDZ from start of block until initialization                                 | No TDZ, accessible immediately after hoisting                                                    |

*Let's see the first difference (scope) in an example:*

Consider these two JavaScript functions:
```js
function varScoping() {
  var x = 1;

  if (true) {
    var x = 2;
    console.log(x); // will print 2
  }

  console.log(x); // will print 2
}

function letScoping() {
  let x = 1;

  if (true) {
    let x = 2;
    console.log(x); // will print 2
  }

  console.log(x); // will print 1
}
```

In `varScoping()`, one `x` variable is used throughout the function, even though an `x` variable is declared in two different places with different values.

In `letScoping()`, two distinct `x` variables are used – one appears in the main function body and another in the `if` block. This behavior remains the same if we replace the first `let` keyword with a `var` keyword:
```js
function varAndLetScoping() {
  var x = 1;

  if (true) {
    let x = 2;
    console.log(x); // will print 2
  }

  console.log(x); // will print 1
}
```

A `var` variable will be available thoroughout the function body in which it is defined, no matter how deeply nested its definition. A `let` variable will only be available within the same block where it is defined. See below:
```js
function nestedScopeTest() {
    if (true) {
        var functionVariable = 1;
        let blockVariable = 2;

        console.log(functionVariable); // 1
        console.log(blockVariable); // 2

        if (true) {
            console.log(functionVariable); // 1
            console.log(blockVariable); // 2
        }
    }

    console.log(functionVariable); // 1
    console.log(blockVariable); // ReferenceError: blockVariable is not defined
}
```

This works because the `var` declaration of `functionVariable` is [[Hoisting|hoisted]] to the top level of `nestedScopeTest()` before execution, but the `let` declaration of `blockVariable` is not.

The behavior of `var` can be useful in some cases, but is quite different from other programming languages, and can cause difficult-to-resolve bugs. The more recently introduced `let` keyword allows for more precise and predictable variable scoping, and allows programmers to safely reuse names for temporary variables within the same function.

One final point to note is that when working outside of function bodies, at a global level, `let` does not create a property on the [global object](https://developer.mozilla.org/en-US/docs/Glossary/Global_object), whereas `var` does. Therefore:
```js
// Global variables
var x = 1;
let y = 2;
console.log(this.x); // will print 1
console.log(this.y); // will print undefined
```

#### Why were `let`/`const` introduced?
- No surprises. `var` behaves differently based on where it's used, has less intuitive scoping which is a source of unwelcome surprises, and you can even use `var` for names already declared.
- `let` and `const` are consistent, have more commonly known scoping rules.
- No confusion with hoisting
- More predictable and safer behaviour compared to `var`

#### Difference between let and const
There are two main differences between `let` and `const`:

| Feature                         | `let`                                     | `const`                                      |
|---------------------------------|-------------------------------------------|----------------------------------------------|
| **Reassignment**                | Can be reassigned                         | Cannot be reassigned after initial declaration |
| **Initialization Requirement**  | Can be declared without an initial value  | Must be initialized at the time of declaration |


> [!NOTE] let / const and Global object
> Variables declared with `let` and `const` are stored in a separate memory space. Not in the global object. 
> 
> This means they are not a property of the global object ( unlike in case of `var`)

#### Reassigning Variables
```js
let test = "XYZ";
console.log(test); // XYZ
test = "ABC";
console.log(test); // ABC
```

If we try to reassign `const` we will get the following `TypeError`:

> [!danger] Uncaught TypeError: Assignment to constant variable.

#### Shadowing
**Shadowing** in JavaScript occurs when a variable declared in an inner scope (such as a function or block) has the same name as a variable in an outer scope. The inner variable "shadows" or overrides the outer variable within its own scope, meaning that references to the variable name in the inner scope will refer to the inner variable, effectively hiding the outer variable.
##### Shadowing with let/const
```js
let message = "Hello from global scope!";

{
    let message = "Hello from block scope!"; // Shadows the outer 'message'
    console.log(message); // Logs: "Hello from block scope!"
}

console.log(message); // Logs: "Hello from global scope!"
```

In this example:

- The global variable `message` is defined with the value `"Hello from global scope!"`.
- Inside the block, another `message` variable is defined within the block scope, with the value `"Hello from block scope!"`.
- The function logs `"Hello from block scope!"`, as it references the local `message` variable instead of the global one.
- Outside of the function, `console.log(message);` refers to the global `message`, which is unchanged.
##### Shadowing with var
```js
var message = "Hello from global scope!";

{
    var message = "Hello from block scope!"; // Shadows the outer 'message'
    console.log(message); // Logs: "Hello from block scope!"
}

console.log(message); // Logs: "Hello from block scope!"
```
*In this example:*
- **Global Scope Declaration**:
    - `var message = "Hello from global scope!";` declares a global variable `message` and initializes it with `"Hello from global scope!"`.
- **Block Scope (But Using `var`)**:
    - Inside the block `{}`, we declare `var message = "Hello from block scope!";`.
    - Since `var` is **not block-scoped**, this declaration does not create a new variable limited to the block. Instead, it **reassigns** the existing global variable `message` to `"Hello from block scope!"`.
- **First `console.log(message);` Inside the Block**:
    - When `console.log(message);` is called inside the block, it logs `"Hello from block scope!"`.
    - This is because `message` has been reassigned globally to `"Hello from block scope!"` within this block.
- **Second `console.log(message);` Outside the Block**:
    - When we log `message` outside the block, it still outputs `"Hello from block scope!"`.
    - Since `var` is function-scoped (not block-scoped), the block assignment affected the global `message` variable itself.
    - So, the change made inside the block persists outside the block, and we see `"Hello from block scope!"` again.

##### Illegal Shadowing
Illegal shadowing occurs in JavaScript when you attempt to declare a variable with `var` that has the same name as a variable already declared with `let` or `const` in a parent scope.
```js
let a = "Global Scope";

{
    var a = "Block Scope"; // ❌ SyntaxError: Identifier 'a' has already been declared
}
```

This happens because any variable shadowing it's parent's variable should not cross the boundary of its parent. Since `var` is function scoped while `let` is block scoped, the inner scope is in a way trying to re-declare the variable `a` which is not allowed. If the block were within a function, it would have been valid.
```js
let a = "Global Scope";

function b() {
    var a = "Function Scope";
    console.log(a); // ✅ Logs: "Function Scope"
}

b();
```

The inverse (declare a variable with `let` or `const` that has the same name as a variable already declared with `var` in a parent scope) is also valid.
```js
const a = "Global Scope";

{
    let a = "Block Scope"; // ✅ logs: "Block Scope"
    console.log(a);
}

console.log(a); // ✅ logs: "Global Scope"
```

#### Data Types
##### Primitive Data Types
###### Number
```js
let age = 25;
console.log(typeof age); // number
```
###### String
```js
let string = "Alice";
console.log(typeof string); // string 
```

###### BigInt
```js
let age = BigInt(25);
console.log(typeof age); // bigint
```

###### Boolean
```js
let booleans = true;
console.log(typeof booleans); // boolean
```

###### Undefined
```js
let undefined_var;
console.log(typeof undefined_var); // undefined
```

###### Null
The **`null`** value represents the intentional absence of any object value. It is one of JavaScript's [primitive values](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) and is treated as [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) for boolean operations.
```js
let null_var = null;
console.log(typeof null_var); // object
```

> [!NOTE] 
> Even though `typeof(null)` returns `object`, `null` is a primitive data type.

###### Symbol
```js
let symbol = Symbol("symbol");
console.log(typeof symbol); // symbol
```

##### Non-Primitive Data Types
###### Object
```js
let obj = {
    firstName: "John",
    lastName: "Doe",
    age: 30
};
console.log(typeof obj); // object
```

###### Array
```js
let array = ["apple", "banana", "cherry"];
console.log(typeof array);  // object
```

###### Function
```js
let greet = function() {
    return "Hello, world!";
};
console.log(typeof greet); // function
```

> [!NOTE]
> Even though `typeof(function)` returns `function`, functions are objects.


#### null vs undefined vs not defined

| Term          | Description                                                                                                                                                                                                                                                                                                               |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| null          | Represents an intentional absence of any object or value. It is explicitly assigned to a variable to indicate "no value" or "nothing." Type is `object`.                                                                                                                                                       |
| undefined     | Automatically assigned to a variable by JavaScript when it is declared but not yet assigned a value. Type is `undefined`. Although it is possible to assign `undefined` manually, it's not considered good practice because `undefined` is typically used by JavaScript to indicate an uninitialized variable. |
| "not defined" | Refers to an undeclared variable or identifier. Trying to access a variable or identifier that has never been declared results in a `ReferenceError` saying it is "not defined."                                                                                                                               |

