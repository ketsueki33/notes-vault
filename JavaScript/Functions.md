Generally speaking, a function is a "subprogram" that can be _called_ by code external (or internal, in the case of recursion) to the function. Like the program itself, a function is composed of a sequence of statements called the _function body_. Values can be _passed_ to a function as parameters, and the function will _return_ a value.

In JavaScript, functions are [first-class objects](https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function), because they can be passed to other functions, returned from functions, and assigned to variables and properties. They can also have properties and methods just like any other object. What distinguishes them from other objects is that functions can be called.

#### Function Declaration
syntax:
```js 
function myFunction(g1, g2) {
    return g1 / g2;
}
const value = myFunction(8, 2); // Calling the function
console.log(value);
```

ex) write a function to return the square of the input number.
```js
function square(x) {
    return x * x;
}
```

#### Function Expression
The `function` keyword can be used to define a function inside an expression.

A `function` expression is very similar to, and has almost the same syntax as, a [[Functions#Function Declaration|Function declaration]]. The main difference between a `function` expression and a `function` declaration is the _function name_, which can be omitted in `function` expressions to create _anonymous_ functions. A `function` expression can be used as an [IIFE](https://www.udacity.com/blog/2023/03/immediately-invoked-function-expressions-iife-in-javascript.html) (Immediately Invoked Function Expression) which runs as soon as it is defined.


> [!INFO] Function expression and Hoisting
>  Function expressions in JavaScript are not hoisted, unlike function declarations. You can't use function expressions before you create them:
>  ```js
>  console.log(notHoisted); // undefined
> // Even though the variable name is hoisted,
> // the definition isn't. so it's undefined.
> notHoisted(); // TypeError: notHoisted is not a function
> 
> var notHoisted = function () {
>   console.log("bar");
> };
> ```

Ex) Write a function expression to concatenate two strings and return it.
```js
const concat = function (s1, s2) {
    return s1 + s2;
};
```

#### Arrow Functions
An Arrow Function in JavaScript, introduced in ES6, offers a concise syntax for defining function expressions using =>. It maintains lexical `this` binding ([['this' keyword#Arrow Functions and 'this'|see here]]) and provides shorter, more readable code compared to traditional functions. Arrow functions enhance code structure by simplifying function syntax without sacrificing functionality or clarity.

**Arrow functions** are anonymous functions i.e. functions without a name but they are often assigned to any variable. They are also called **Lambda Functions**.

ex) Write an arrow function to check if a string contains a specific character.
```js
const checkString = (s, c) => {
    return s.includes(c) ? true : false;
};
```

#### Difference between Regular Functions and Arrow Functions

| Feature           | Regular Function                                                                                                                  | Arrow Function                                                                                                                  |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `this` Value      | `this` is dynamic and depends on how the function is invoked: global object, method owner, context of call/apply, or new instance | the `this` keyword refers to the object from which you define the function and remains constant regardless of invocation method |
| Constructors      | Can be used as constructors to create new object instances (e.g., `new Car()`)                                                    | Cannot be used as constructors; attempting to use `new` with an arrow function results in a `TypeError`                         |
| `arguments`Object | Has a special `arguments` object that contains all passed arguments                                                               | Does not have an `arguments` object; accesses arguments lexically from the outer function or uses rest parameters (`...args`)   |
| Implicit Return   | Requires an explicit `return` statement to return values; otherwise returns `undefined`                                           | Allows implicit return for single expressions when curly braces are omitted                                                     |
| Methods           | Regular functions are used to define methods in classes or objects, but can lose `this` context when used as callbacks            | Arrow functions bind `this` lexically, ensuring `this` context is maintained when used as methods or callbacks                  |
For more in-depth differences: [refer this](https://dmitripavlutin.com/differences-between-arrow-and-regular-functions/)

#### Default Parameters
Default parameter values allow you to initialize parameters with default values if no value or `undefined` is passed when the function is called. This feature helps make your code more robust and reduces the need for additional checks for `undefined` values.

ex) write a function that takes two parameters and returns their product. Provide a default value for the second parameter.
```js
const product = (a, b = 5) => {
    return a * b;
};
```

#### Higher Order Functions
A higher order function is a function that takes one or more functions as arguments, or returns a function as its result.

Some built-in higher order functions include: `map()`,`reduce()`, `filter()`, `sort()`, `entries()`,  `compose()` etc.

Ex) Write a higher order function that takes a function and a number, and calls that function that many times.
```js
const repeatCall = (func, x) => {
    while (x--) {
        func();
    }
};

repeatCall(() => console.log("Yo"), 5);
```

Ex) Write a higher order function that takes two functions and a value, applies the first function to the value, and then applies the second function to the result.
```js
function compose(func1, func2) {
    return function (value) {
        const result1 = func1(value);
        const result2 = func2(result1);
        return result2;
    };
}

function double(x) {
    return x * 2;
}

function square(x) {
    return x * x;
}

const doubleThenSquare = compose(double, square);

const result = doubleThenSquare(10); // First doubles 5 to get 10, then squares 10 to get 100

console.log(result); // Output: 100
```

#### Constructor Functions and Factory Functions

see: 
[[Objects#Constructor Function|Constructor Function]]   

[[Objects#Factory Functions|Factory Functions]]

