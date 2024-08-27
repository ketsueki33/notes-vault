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

The difference between `let` and `var` is in the **scope** of the variables they create:

- Variables declared by `let` are **only available inside the block** where they’re defined.
- Variables declared by `var` are **available throughout the function** in which they’re declared.

Consider the difference between these two JavaScript functions:
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

    console.log(functionVariable); // will print 1
    console.log(blockVariable); // will print 2

    if (true) {
      console.log(functionVariable); // will print 1
      console.log(blockVariable); // will print 2
    }
  }

  console.log(functionVariable); // will print 1
  console.log(blockVariable); // will throw a reference error
}
```

This works because the `var` declaration of `functionVariable` is [hoisted](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting) to the top level of `nestedScopeTest()` before execution, but the `let` declaration of `blockVariable` is not.

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

#### Reassigning Variables
```js
let test = "XYZ";
console.log(test); // XYZ
test = "ABC";
console.log(test); // ABC
```

If we try to reassign `const` we will get the following `TypeError`:

> [!danger] Uncaught TypeError: Assignment to constant variable.

