The `this` keyword in JavaScript refers to the context in which a function is called. Its value can vary depending on how a function is invoked.

#### How `this` Works in Different Contexts?
##### Global Context 
In the global context, `this` refers to the global object (`window` in browsers or `global` in Node.js).
```js
console.log(this); // Window (in browsers)
```

##### Function Context
When a function is called in the global context, `this` refers to the global object.
```js
function showThis() {
    console.log(this);
}
showThis(); // Window (in browsers)
```

##### Method Context
When a method is called on an object, this refers to the object itself.
```js
const obj = {
    value: 42,
    showThis: function() {
        console.log(this);
    }
};
obj.showThis(); // { value: 42, showThis: [Function: showThis] }
```

##### Constructor Context
When a function is used as a constructor with the new keyword, this refers to the newly created object.
```js
function Person(name) {
    this.name = name;
}
const person1 = new Person('Bob');
console.log(person1.name); // Bob
```

##### Event Handler Context
In an event handler, this refers to the element that received the event.
```js
document.getElementById('myButton').addEventListener('click', function() {
    console.log(this); // <button id="myButton">...</button>
});
```

#### Arrow Functions and 'this'
[[Functions#Arrow Functions|Arrow functions]] have a different behavior when it comes to `this`. They do not have their own `this`context; instead, they inherit `this` from the enclosing lexical context.

```js
const obj = {
    name: 'Carol',
    greet: function() {
        const innerFunc = () => {
            console.log(this.name);
        };
        innerFunc();
    }
};

obj.greet(); // Carol
```

In this example, the arrow function `innerFunc` inherits `this` from the `greet` method, which means it refers to the `obj` object.

**Practical Example**
```js
const company = {
    name: 'Tech Corp',
    employees: [
        { name: 'Alice', position: 'Developer' },
        { name: 'Bob', position: 'Designer' }
    ],
    showEmployees: function() {
        this.employees.forEach(function(employee) {
            console.log(`${this.name} employs ${employee.name} as a ${employee.position}`);
        });
    }
};

company.showEmployees();
// Output:
// undefined employs Alice as a Developer
// undefined employs Bob as a Designer
```

In the above example, `this.name` inside the `forEach` callback does not refer to `company.name` because `this` inside the callback refers to the global object.

To fix this, you can use an arrow function:
```js
const company = {
    name: 'Tech Corp',
    employees: [
        { name: 'Alice', position: 'Developer' },
        { name: 'Bob', position: 'Designer' }
    ],
    showEmployees: function() {
        this.employees.forEach(employee => {
            console.log(`${this.name} employs ${employee.name} as a ${employee.position}`);
        });
    }
};

company.showEmployees();
// Output:
// Tech Corp employs Alice as a Developer
// Tech Corp employs Bob as a Designer
```

Here, the arrow function correctly inherits `this` from the `showEmployees` method.

#### 'this' keyword in Arrow Functions v/s Regular Functions

##### 1. Standalone Functions
```js
function regular() {
    console.log(this); 
}

regular(); // window/global
```
 
When called as a standalone function (not as a method, not with new, not with call/apply), `this` will default to the global object ( `window` in browser). In strict mode, however, `this` would be undefined.

---

```js
const arrow = () => {
    console.log(this);
};

arrow(); // window( browser) / {} (nodeJS)
```
As we know, the arrow function inherits `this` from the enclosing scope where it's defined. If we run this code in NodeJs (module scope), `this` is `{}`(empty object). If we run this code in browser (global scope), `this` would be the global object (i.e `window`).

##### 2. Object Methods
```js
const obj1 = {
    name: "Carol",
    greet: function () {
        console.log(this.name);
    },
};

obj1.greet(); // Carol
```
When calling a function as a method of an object, `this` will refer to the object owning the method. Hence, `greet` function logs the name from the owner object.

---

```js
const obj2 = {
    name: "Carol",
    greet: () => {
        console.log(this.name);
    },
};

obj2.greet(); // undefined
```


Arrow function will inherit `this` from the enclosing lexical scope and that is the module scope( in case of NodeJS) or global scope (in case of browser). Either way the arrow function will log `undefined` because `name` property does not exist in either scope. In case of browser though, if you create a `name` variable using `var` , then the arrow function would log that name.

##### 3. Inner Function of Object Methods
```js
const obj3 = {
    alias: "Carol",
    greet: function () {
        function innerFunc() {
            console.log(this.alias);
        }
        innerFunc();
    },
};

obj3.greet(); // undefined
```

`this` inside `innerFunc()` refers to the global object (or `undefined` in strict mode) because even though `innerFunc` is defined inside `greet` method, when it's called as a standalone function (`innerFunc()`), it loses the context of `obj`.

---

```js
const obj4 = {
    alias: "Carol",
    greet: function () {
        const innerFunc = () => {
            console.log(this.alias);
        };
        innerFunc();
    },
};

obj4.greet(); // Carol
```

The arrow function's enclosing lexical scope is the `greet` function's scope and it will inherit `this` from that scope. Hence, `this` here will refer to the object that owns the `greet` method.

#### call, apply and bind methods
In JavaScript, the value of `this` is determined by how a function is called, not by where it is defined (the lexical context). This is known as dynamic binding of `this`.

The `call()`, `apply()`, and `bind()` methods allow you to explicitly set the value of `this` when calling a function, overriding the default dynamic binding. 

 Every function in JS has access to these methods.
##### call( )
Using `call()` method, an object can borrow functions from other objects. This is also called method borrowing. It can also be used for reusing the same function for multiple objects.

```js
const person = {
    name: "John",
    greet: function (message, punctuation) {
        return `${message}, I'm ${this.name}${punctuation}`;
    },
};

const anotherPerson = {
    name: "Jane",
};

console.log(person.greet.call(anotherPerson, "Hi", "!"));
// Output: "Hi, I'm Jane!"
```

We use the `call` method of the function we want to borrow and pass the lexical context as the first argument and any other argument that the function requires as separate arguments. If we pass less arguments than what the function is expecting, the remaining arguments will be `undefined`. ( just like for normal functions)

*`call()` executes the function immediately.*

##### apply( )
`apply()` method is the same as `call()` method but takes arguments as an array instead of individual arguments.
```js
const person = {
    name: "John",
    greet: function (message, punctuation) {
        return `${message}, I'm ${this.name}${punctuation}`;
    },
};

const anotherPerson = {
    name: "Jane",
};

console.log(person.greet.apply(anotherPerson, ["Hello", "?"]));
// Output: "Hello, I'm Jane?"
```


*`apply()` executes the function immediately.*

##### bind( )
`bind()` method instead of executing the function with the given context, binds the context to the method and returns a new method (with that fixed `this`).

You can also partially apply arguments with bind.
```js
const person = {
    name: "John",
    greet: function (message, punctuation) {
        return `${message}, I'm ${this.name}${punctuation}`;
    },
};

const anotherPerson = {
    name: "Jane",
};

const janeGreet = person.greet.bind(anotherPerson);
console.log(janeGreet("Hey", "."));
// Output: "Hey, I'm Jane."

//partially apply arguments with bind
const janeGreetHi = person.greet.bind(anotherPerson, 'Hi');
console.log(janeGreetHi('!')); 
// Output: "Hi, I'm Jane!"
```

**Why was bind() method important:**
Before the introduction of arrow functions in ES6, the `bind()` method played a crucial role in managing the `this` context in JavaScript.

The dynamic binding of `this` within functions sometimes lead to unexpected behaviour, especially when working with callbacks, event handlers or object methods.

Without arrow functions, the only way to reliably maintain the desired `this` value was to use the `bind()` method.

##### Type of Lexical Contexts that can be passed

###### 1. Object Context
You can pass an object as the `this` value, which makes the function behave as if it was a method of that object.
```js
const person = {
  name: 'John',
  greet: function() {
    console.log(`Hello, my name is ${this.name}`);
  }
};

person.greet(); // "Hello, my name is John"
person.greet.call({name: 'Jane'}); // "Hello, my name is Jane"
```

###### 2. Primitive Values
You can also pass primitive values (strings, numbers, booleans, etc.), and they will be converted to their corresponding wrapper objects.
```js
function printThis() {
  console.log(this);
}

printThis.call(42); // Number {42}
printThis.call('hello'); // String {'hello'}
```

###### 3. null or undefined
If you pass `null` or `undefined`, the `this` value will be set to the global object (in a browser, this is the `window` object).
```js
function printThis() {
  console.log(this);
}

printThis.call(null); // Window {...} / Global {...}
printThis.call(undefined); // Window {...} / Global {...}
```

