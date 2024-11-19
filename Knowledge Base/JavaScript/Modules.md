A module is a function or group of similar functions. They are grouped together within a file and contain the code to execute a specific task when called into a larger application.

You create modules to better organize and structure your code base. You can use them to break down large programs into smaller, more manageable, and more independent chunks of code which carry out a single or a couple of related tasks.

Modules should be:

1. **Independent/Self-contained:** A module has to be as detached from other dependencies as possible.
2. **Specific:** A module needs to be able to perform a single or a related group of tasks. The core essence of creating them in the first place is to create separate functionalities. One module, one (kind of) task.
3. **Reusable:** A module has to be easy to integrate into various kinds of programs to perform its task.

#### CommonJS vs ES6 Modules
In JavaScript, there are important differences between how exports are handled in traditional scripts (commonJS) and in ES6 modules. These differences relate to the scope, syntax, and behavior of the code.

##### CommonJS
CommonJS is a module system used by default in Node.js. It uses `module.exports` and `require()` to export and import modules.
```js
//module.js
const name = 'John Doe';
const age = 30;

function greet() {
    console.log(`Hello, my name is ${name} and I am ${age} years old.`);
}

module.exports = { name, age, greet };
//----------------------------------------------
//app.js
const { name, age, greet } = require('./module');

console.log(name); // Outputs: John Doe
console.log(age);  // Outputs: 30
greet();           // Outputs: Hello, my name is John Doe and I am 30 years old.
```

##### ES6 Modules
ES6 introduced a built-in module system. ES6 modules have several advantages over traditional scripts:
1. **Static Analysis**: ES6 modules are statically analyzable, meaning that the dependencies are known at compile time.
2. **Strict Mode**: ES6 modules are always in strict mode.
3. **Scope**: Variables declared inside a module are scoped to that module and do not pollute the global scope.
4. **Syntax**: ES6 modules have a clean and concise syntax for exporting and importing.

In ES6 and above, we use the `import` and `export` keywords to share and receive functionalities respectively across different modules.
- The `export` keyword is used to make a variable, function, class or object  accessible to other modules. In other words, it becomes a public code.
- The `import` keyword is used to bring in public code from another module.

```js
// person.js
const person = {
    name: "John Doe",
    age: 30,
    greet() {
        console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
    },
    incrementAge() {
        this.age += 1;
        console.log(`I am now ${this.age} years old.`);
    },
};

export { person };
//----------------------------------------------
// app.js
import { person } from "./person.js";

person.greet(); // Outputs: Hello, my name is John Doe and I am 30 years old.
person.incrementAge(); // Outputs: I am now 31 years old.
person.greet(); // Outputs: Hello, my name is John Doe and I am 31 years old.

```


> [!Important] Note
> ` Node` along with a lot of libraries support ES6 modules .. so the rest of the chapter will only cover ES6 modules. To use ES6 modules in `Node`.. add the following to `package.json`
> ```json
> "type": "module"
> ```

#### Named and Default Imports
##### Named Imports
Named exports allow you to export multiple values from a module. Each value is exported by its name. *When importing, you must use the same name to reference the exported value.*

```js
// module.js (with named imports)
export function greetUser(username) {
    return `Hello , ${username}`;
}

export function sayBye(username) {
    return `Bye bye , ${username}`;
}

export function returnRandomNumber() {
    const randomNumber = Math.floor(Math.random() * 100 + 1);
    return randomNumber;
}
```
```js
// app.js
import { greetUser, returnRandomNumber, sayBye } from "./module.js";

console.log(returnRandomNumber()); // 33
console.log(greetUser(`ketsueki`)); // Hello , ketsueki
console.log(sayBye(`ketsueki`));// Bye bye , ketsueki
```

**You can also import everything as an object:**
```js
// app.js
import * as functions from "./module.js";

console.log(functions.returnRandomNumber()); // 63
console.log(functions.greetUser(`ketsueki`)); // Hello , ketsueki
console.log(functions.sayBye(`ketsueki`)); // Bye bye , ketsueki
```

##### Default Imports
Default exports allow you to export a single value or function as the default export of a module. Each module can only have one default export. *When importing, you can use any name to reference the default export.*
```js
// module.js
function whoWon() {
    return "DU DU DU DU MAX VERSTAPPEN!";
}

export default whoWon;
```
```js
// app.js
import someFunction from "./module.js";

console.log(someFunction()); // DU DU DU DU MAX VERSTAPPEN!
```


> [!INFO] Note
> You can use both named and default exports in the same module.
> ```js
> // module.js
> export const name = 'John Doe';
> export const age = 30;
> 
> const person = {
>     name,
>     age,
>     greet() {
>         console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
>     }
> };
> 
> export default person;
> ```
> ```js
> // app.js
> import person, { name, age } from './module.js';
> 
> console.log(name); // Outputs: John Doe
> console.log(age);  // Outputs: 30
> person.greet();    // Outputs: Hello, my name is John Doe and I am 30 years old.
> ```


#### Third Party Modules (Node.js)
Using third-party modules in Node.js is a common practice to leverage pre-built functionality and avoid reinventing the wheel. Node.js has a vast ecosystem of modules, thanks to the Node Package Manager (npm).

Make sure you have initialized a Node.js project (with `npm init`). To install a third-party module, use the `npm install` command followed by the name of the module. When you install a third-party module, it is added to the `dependencies` section of your `package.json` file.

```bash
npm install pokemon
```

And we can use the module in our application:
```js
import pokemon from "pokemon";

console.log(pokemon.getName(3)); // Venusaur

console.log(pokemon.getId("Rayquaza")); // 384
```

#### Module Bundling
Module bundling in JavaScript is the process of combining multiple JavaScript modules into a single, cohesive file (or a few files) that can be efficiently loaded by the browser. This is especially important for web development, as it helps manage dependencies, optimize load times, and improve performance.

*Advantages of Module Bundling:*
- **Dependency Management**: Modern JavaScript applications often consist of many small modules that depend on each other. Bundlers help manage these dependencies and ensure they are loaded in the correct order.
- **Performance Optimization**: By combining multiple modules into a single file, bundlers reduce the number of HTTP requests needed to load a web page, which can significantly improve load times.
- **Code Splitting**: Bundlers can split code into smaller chunks that can be loaded on demand, further optimizing performance.
- **Minification and Tree Shaking**: Bundlers can also minimize the size of the bundled files by removing unused code (tree shaking) and compressing the code (minification).

Popular Module Bundlers: Webpack, Parcel, Rollup