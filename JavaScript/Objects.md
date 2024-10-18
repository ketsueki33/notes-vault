The **`Object`** type represents one of [JavaScript's data types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures). It is used to store various keyed collections and more complex entities.

#### Creating an Object 
##### Object Literal
The most straightforward way to create an object is by using an object literal.
```js
const obj = {
    key1: 'value1',
    key2: 'value2',
    method: function() {
        console.log('Hello!');
    }
};
console.log(obj); // { key1: 'value1', key2: 'value2', method: [Function: method] }
```

##### Constructor Function
You can define a constructor function and use the `new` keyword to create instances of an object.

A constructor is a special function that creates and initializes an object instance of a class. In JavaScript, a constructor gets called when an object is created using the `new` keyword. The function name begins with a capital letter to denote constructor functions.

The purpose of a constructor is to create a new object and set values for any existing object properties.
```js
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.greet = function() {
        console.log(`Hello, my name is ${this.name}`);
    };
}

const person1 = new Person('John', 30);
console.log(person1); // Person { name: 'John', age: 30, greet: [Function] }
```

##### ES6 Classes
ES6 introduced [[Classes (OOP)|classes]], which provide a clearer and more concise syntax to create objects and deal with inheritance.
```js
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    greet() {
        console.log(`Hello, my name is ${this.name}`);
    }
}

const person1 = new Person('Jane', 25);
console.log(person1); // Person { name: 'Jane', age: 25 }
```

##### Factory Functions
A factory function is a function that returns a new object.
```js
function createPerson(name, age) {
    return {
        name,
        age,
        greet() {
            console.log(`Hello, my name is ${this.name}`);
        }
    };
}

const person1 = createPerson('Bob', 40);
console.log(person1); // { name: 'Bob', age: 40, greet: [Function: greet] }
```

##### Using Object Constructor
You can create an object using the `Object` constructor.
```js
const obj = new Object();
obj.name = 'Charlie';
obj.age = 28;
console.log(obj); // { name: 'Charlie', age: 28 }
```

#### Accessing Object Properties
Example Object:
```js
const obj = {
    name: "John",
    age: 30,
    address: { street: "123 Main St", city: "New York", country: "USA" },
    hobbies: ["reading", "traveling"],
};
```
##### Dot Notation
You can access properties of nested objects using dot notation(`.`) .
```js
console.log(person.name); // John
console.log(person.address.city); // New York
console.log(person.hobbies[0]); // reading
```

##### Bracket Notation
Bracket notation can also be used, which is especially useful when property names contain spaces or are dynamic.
```js
console.log(person['name']); // John
console.log(person['address']['street']); // 123 Main St
console.log(person['hobbies'][1]); // traveling
```

#### Deleting Object Properties
We can use the delete operator for this.
```js
const details = {
    name: 'Alex',
    age: 30,
    country: 'Canada'
};

console.log('Original Object:', details);
// Original Object: { name: 'Alex', age: 30, country: 'Canada' }

delete details.age;
console.log('Object after deleting age key:', details);
// Object after deleting age key: { name: 'Alex', country: 'Canada' }
```
#### Nested Objects
Nested objects are objects within objects. This allows you to organize and structure data hierarchically. You can access nested objects and their properties using dot notation or bracket notation. 
Example: A library object with property books that is an array of book objects.
```js
let library = {
    name: "Great Library",
    books: [],
};

class Book {
    constructor(title, author) {
        this.title = title;
        this.author = author;
    }
}

library.books.push(new Book("Harry Potter", "Trans Hating Rowling"));
library.books.push(new Book("1984", "George Orwell"));

console.log(library);
// {
//   name: 'Great Library',
//   books: [
//     Book { title: 'Harry Potter', author: 'Trans Hating Rowling' },
//     Book { title: '1984', author: 'George Orwell' }
//   ]
// }

library.books.forEach((ele) => console.log(ele.title));
// Harry Potter
// 1984
```

#### Objects and [['this' keyword|'this']] keyword 
The `this` keyword in JavaScript refers to the context in which a function is called. Its value can vary depending on how a function is invoked. When used within an object, this refers to the object itself, allowing you to access its properties and methods from within its methods.
```js
const person = {
    name: 'Alice',
    age: 28,
    greet: function() {
        console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
    },
    updateAge: function(newAge) {
        this.age = newAge;
    }
};

person.greet(); // Hello, my name is Alice and I am 28 years old.
person.updateAge(29);
person.greet(); // Hello, my name is Alice and I am 29 years old.
```

#### Object Iteration

##### Using `for...in`
The `for...in` statement iterates over all enumerable properties of an object, including properties inherited from the prototype chain.
```js
const person = {
    name: 'John',
    age: 30,
    occupation: 'Developer'
};

for (const key in person) {
    console.log(`${key}: ${person[key]}`);
}

// Output:
// name: John
// age: 30
// occupation: Developer
```

*Characteristics:*
- Iterates over all enumerable properties, including inherited properties.
- Not recommended for arrays, as it may iterate over non-index properties and inherited properties.

##### Using `Object.keys()`

The `Object.keys()` method returns an array of the object's own enumerable property names (keys).
```js
const person = {
    name: 'John',
    age: 30,
    occupation: 'Developer'
};

const keys = Object.keys(person);
console.log(keys);
// [ 'name', 'age', 'occupation' ]

keys.forEach(key => {
    console.log(`${key}: ${person[key]}`);
});

// Output:
// name: John
// age: 30
// occupation: Developer
```

*Characteristics:*
- Returns only the object's own enumerable properties, not inherited ones.
- Suitable for iterating over an object's properties when you don't want to include properties from the prototype chain.

##### Using `Object.values()`
The `Object.values()` method returns an array of the object's own enumerable property values.
```js
const person = {
    name: 'John',
    age: 30,
    occupation: 'Developer'
};

const values = Object.values(person);
console.log(values);
// [ 'John', 30, 'Developer' ]

values.forEach(value => {
    console.log(value);
});

// Output:
// John
// 30
// Developer
```

*Characteristics:*
- Returns only the object's own enumerable property values.
- Useful when you need the values of the properties without the keys.

##### Using `Object.entries()`
The `Object.entries()` method returns an array of the object's own enumerable property `[key, value]` pairs.
```js
const person = {
    name: 'John',
    age: 30,
    occupation: 'Developer'
};

const entries = Object.entries(person);
console.log(entries);
// [ [ 'name', 'John' ], [ 'age', 30 ], [ 'occupation', 'Developer' ] ]

entries.forEach(([key, value]) => {
    console.log(`${key}: ${value}`);
});

// Output:
// name: John
// age: 30
// occupation: Developer
```
