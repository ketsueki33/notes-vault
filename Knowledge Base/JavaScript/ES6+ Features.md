#### Template Literals
Template literals in JavaScript are a way to work with strings that allows for easier embedding of expressions and multi-line strings. They use backticks (`` ` ``) instead of single or double quotes.

##### Expression Interpolation:
Template literals allow embedding of expressions inside the string using `${ }` syntax.
```js
const name = 'John';
const age = 30;
const greeting = `Hello, my name is ${name} and I am ${age} years old.`;
console.log(greeting);
// Output: Hello, my name is John and I am 30 years old.
```

##### Multi-line Strings:
Template literals can span multiple lines without needing escape characters for newlines.
```js
const multiLineString = `This is a string
that spans multiple
lines.`;
console.log(multiLineString);
// Output:
// This is a string
// that spans multiple
// lines.
```

##### Tagged Templates:
Tagged template literals allow you to parse template literals with a function.

Tags allow you to parse template literals with a function. The first argument of a tag function contains an array of string values. The remaining arguments are related to the expressions.

The tag function can then perform whatever operations on these arguments you wish, and return the manipulated string. (Alternatively, it can return something completely different )

```js
const person = "Max";
const age = 25;

function tagExample(strings, personExp, ageExp) {
    console.log("strings: ", strings);
    const str0 = strings[0];
    const str1 = strings[1];
    const str2 = strings[2];

    const ageStr = ageExp < 18 ? "teenager" : "adult";

    return str0 + personExp + str1 + ageStr + str2;
}

const output = tagExample`That is ${person}, a ${age}.`;

console.log(output);
// Output:
// strings:  [ 'That is ', ', a ', '.' ]
// That is Max, a adult.
```

#### Destructuring
Destructuring in JavaScript is a convenient way of extracting values from arrays or properties from objects into distinct variables.

##### Array Destructuring
```js
const [a, b] = [1, 2];
console.log(a); // 1
console.log(b); // 2
```

###### Skipping Values:
```js
const [first, , third] = [1, 2, 3];
console.log(first); // 1
console.log(third); // 3
```

###### Rest Syntax:
You can use the rest syntax to collect the remaining elements into an array.
```js
const [head, ...tail] = [1, 2, 3, 4];
console.log(head); // 1
console.log(tail); // [2, 3, 4]
```

###### Default Values:
You can assign default values to variables if the array does not contain that element.
```js
const [a = 1, b = 2, c = 3] = [7];
console.log(a); // 7
console.log(b); // 2
console.log(c); // 3
```

###### Swapping variables:
Two variables values can be swapped in one destructuring expression.
```js
let a = 1;
let b = 3;

[a, b] = [b, a];
console.log(a); // 3
console.log(b); // 1
```

###### Extra Variables:
In an array destructuring from an array of length _N_ specified on the right-hand side of the assignment, if the number of variables specified on the left-hand side of the assignment is greater than _N_, only the first _N_ variables are assigned values. The values of the remaining variables will be undefined.
```js
const [red, yellow, green, blue] = ["one", "two"];
console.log(red); // "one"
console.log(yellow); // "two"
console.log(green); // undefined
console.log(blue); // undefined
```

##### Object Destructuring
```js
const person = { name: 'Alice', age: 25 };
const { name, age } = person;
console.log(name); // Alice
console.log(age); // 25
```

###### Renaming Variables:
You can rename variables while destructuring.
```js
const person = { name: 'Alice', age: 25 };
const { name: fullName, age: years } = person;
console.log(fullName); // Alice
console.log(years); // 25
```

###### Default Values:
You can provide default values for variables in case the property does not exist in the object.
```js
const person = { name: 'Alice', age: 25 };
const { name, age, gender = 'unknown' } = person;
console.log(gender); // unknown
```

###### Renaming along with default values
```js
const person = { name: "Alice", age: 25 };
const { name, age, gender: sex = "unknown" } = person;
console.log(sex); // unknown
```
###### Nested Destructuring:
```js
const user = {
  id: 42,
  details: {
    name: 'Alice',
    age: 25,
  },
};

const { id, details: { name, age } } = user;
console.log(id); // 42
console.log(name); // Alice
console.log(age); // 25
```

###### Rest Properties:
You can collect the remaining properties into a new object.
```js
const { name, ...rest } = person;
console.log(name); // Alice
console.log(rest); // { age: 25 }
```

#### Spread and Rest Operators
In modern JavaScript, the spread and rest operators are indispensable tools for simplifying array manipulation and function parameters. These operators provide elegant solutions for tasks like array expansion and function arguments handling. 

##### Spread Operator
The spread operator, denoted by three consecutive dots `...`, is primarily used for expanding iterables like arrays into individual elements. This operator allows us to efficiently merge, copy, or pass array elements to functions without explicitly iterating through them.
```js
const arr = [1, 2, 3];
console.log("Original array:", arr); // [1, 2, 3]

const newArr = [5, 6, ...arr];
console.log("New array (after spread operator):", newArr); // [5, 6, 1, 2, 3]
```

**Use Cases:**
###### Combining Arrays:
```js
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];
console.log("Combined array:", combined); // [1, 2, 3, 4, 5, 6]
```

###### Passing Arguments to Functions:
```js
function sum(a, b, c) {
    return a + b + c;
}

const nums = [1, 2, 3];
const result = sum(...nums);
console.log("Result of sum:", result); // 6
```

###### Copying Arrays:
The spread operator offers a concise method for copying arrays, ensuring that modifications to the copied array do not affect the original.
```js
const original = [1, 2, 3];
const copy = [...original];
console.log("Copied array:", copy); // [1, 2, 3]
```

##### Rest Operator
While the spread operator expands elements, the rest operator condenses them into a single entity within function parameters or array destructuring. It collects remaining elements into a designated variable, facilitating flexible function definitions and array manipulation.
```js
const [first, ...rest] = [1, 2, 3, 4, 5];
console.log("First element:", first); // 1
console.log("Rest of the elements:", rest); // [2, 3, 4, 5]
```

**Use Cases:**
###### Handling Variable-Length Function Arguments:
```js
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log("Sum:", sum(1, 2, 3, 4, 5)); // Sum: 15
console.log("Sum:", sum(10, 20)); // Sum: 30
```

###### Array Destructuring
The rest operator is commonly used in array destructuring to capture remaining elements into a separate array variable.
```js
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log("First element:", first); // First element: 1
console.log("Second element:", second); // Second element: 2
console.log("Rest of the elements:", rest); // Rest of the elements: [3, 4, 5]
```

#### Enhanced Object Literals

##### Shorthand Property Names
When the property name and the variable name are the same, you can omit the property name.
```js
const name = 'Alice';
const age = 25;

const person = {
  name,
  age
};

console.log(person); // { name: 'Alice', age: 25 }
```

##### Shorthand Method Definitions
You can define methods in objects more concisely without the function keyword.
```js
const person = {
  name: 'Alice',
  age: 25,
  greet() {
    console.log(`Hello, my name is ${this.name}`);
  }
};

person.greet(); // Hello, my name is Alice
```

##### Computed Property Names
You can dynamically compute property names using expressions enclosed in square brackets `[]`.
```js
const key = 'name';
const person = {
  [key]: 'Alice',
  age: 25
};

console.log(person); // { name: 'Alice', age: 25 }
```
