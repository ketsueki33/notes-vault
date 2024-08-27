The `this` keyword in JavaScript refers to the context in which a function is called. Its value can vary depending on how a function is invoked.

#### How `this` Works in Different Contexts?
##### Global Context 
In the global context, `this` refers to the global object (`window` in browsers or `global` in Node.js).
```js
console.log(this); // Window (in browsers)
```

##### Function Context
When a function is called in the global context, this refers to the global object.
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
