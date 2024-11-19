Classes were introduced in EcmaScript 2015 (ES6) to provide a cleaner way to follow object-oriented programming patterns.

JavaScript still follows a prototype-based inheritance model. Classes in JavaScript are syntactic sugar over the prototype-based inheritance model which we use to implement OOP concepts.

> [!INFO] Prototypes in JavaScript
> Prototypes are the mechanism by which JavaScript objects inherit features from one another. Think of it like a template object which contains all the pre-defined methods. 
> 
> We can add our own methods to the prototype so that we can use them across all objects of one type. 
> Eg) Adding a custom method to the string prototype:-
> ```js
> String.prototype.custom = () => {
>     console.log("This a custom method");
> };
> 
> const s = "hello";
> 
> s.custom(); // This a custom method
> ```
> If we define a custom method with the name of a pre-defined method, it will overwrite that method.  Every object has a property `__proto__`. It is a reference to the actual prototype object.


#### Class Definition
Class definition must have a constructor function. It runs when a new object of that class is created.

```js
class Color {
    constructor(r, g, b, name) {
        this.r = r;
        this.g = g;
        this.b = b;
        this.name = name;
    }
    innerRGB() {
        const { r, g, b } = this;
        return `${r}, ${g}, ${b}`;
    }
    rgb() {
        return `rgb(${this.innerRGB()})`;
    }
    rgba(a = 1.0) {
        return `rgba(${this.innerRGB()}, ${a})`;
    }
    hex() {
        const { r, g, b } = this;
        return "#" + ((1 << 24) + (r << 16) + (g << 8) + b).toString(16).slice(1);
    }
}
const red = new Color(255, 67, 89, "tomato");
const white = new Color(255, 255, 255, "white");
```

#### Class Inheritance
Inheritance allows one class to inherit properties and methods from another class using the `extends` keyword.

**extends keyword**
The `extends` keyword is used in class declarations or class expressions to create a class that is a child of another class.

**super keyword**
The super keyword is used to access properties on an object literal or class's `Prototype`, or invoke a super-class's constructor. In other words, it is used to access/call the constructor of the parent class.

```js
class Pet {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    eat() {
        return `${this.name} is eating!`;
    }
}

class Cat extends Pet {
    constructor(name, age, livesLeft = 9) {
        super(name, age);
        this.livesLeft = livesLeft;
    }
    meow() {
        return "MEOWWWW!!";
    }
}

const tom = new Cat("Tom", 2);

console.log(tom.name);// Tom
console.log(tom.livesLeft); // 9
console.log(tom.meow()); // MEOWWWW!!
```

#### Static Methods and Properties
Static methods and properties in JavaScript are used to create functionality that is not tied to an instance of a class but rather to the class itself. **You can use these methods and properties without creating an object instance of the class.**

Static methods are ideal for creating utility functions that perform a task in a self-contained manner without relying on the instance's state.

Static properties are often used to define constants related to a class. This ensures that these values are not duplicated across instances and remain consistent.
```js
class MathUtils {
    static PI = 3.14159;

    static circleArea(r) {
        return this.PI * r * r;
    }
}

console.log(MathUtils.PI); // 3.14159
console.log(MathUtils.circleArea(3)); // 28.274309999999996
```

#### Getters and Setters
Getters and setters allow you to define Object Accessors (Computed Properties).

The `get` accessor cannot have any parameters and  the `set` accessor must have exactly one parameter.
```js
class Rectangle {
    constructor(width, height) {
        this.width = width;
        this.height = height;
    }

    // Getter
    get area() {
        return this.width * this.height;
    }

    // Setter
    set newWidth(value) {
        this.width = value;
    }
    set area(value) {
        throw new Error("Area is a read-only property");
    }
}

const rect = new Rectangle(10, 5);
console.log(rect.area); // 50
rect.newWidth = 5;
console.log(rect.area); // 25
rect.area = 100; // Error: Area is a read-only property
```

##### Example: Calling setter inside constructor
```js
class User {
    constructor(name) {
        this.name = name;
    }
    get name() {
        return this._name;
    }
    set name(newName) {
        this._name = newName.toUpperCase();
    }
}
const user = new User("John");
console.log(user.name); // JOHN
```

The output is "JOHN" because the `User` class uses a getter and a setter for the `name` property, and the setter modifies the name to be in uppercase before storing it.

Here’s a step-by-step explanation:

1. **Class Definition**:
    
    - The `User` class has a constructor that takes a `name` parameter and assigns it to `this.name`.
    - The `name` property has both a getter and a setter.
        - The **getter** returns the value of `this._name`.
        - The **setter** takes `newName`, converts it to uppercase using `newName.toUpperCase()`, and then assigns it to `this._name`.
2. **Creating an Instance**:
    
    - When you create a new instance of `User` with `new User("John")`, the constructor is called with `"John"` as the argument for `name`.
3. **Setter Invocation**:
    
    - Inside the constructor, `this.name = name` invokes the setter `name(newName)`.
    - The setter receives `"John"` as `newName` and sets `this._name` to `"JOHN"` (since `newName.toUpperCase()` is `"JOHN"`).
4. **Getter Invocation**:
    
    - When `console.log(user.name)` is executed, it calls the getter `name()`, which returns `this._name`.
    - `this._name` was set to `"JOHN"` by the setter, so the getter returns `"JOHN"`.

#### Private Fields
Private properties and methods are accessible only within the class they are defined in. **They are declared using the `#` symbol**. Public methods or getters can be used to access these.

```js
class Counter {
    // Private property
    #count = 0;

    // Private method to validate the count
    #validateCount(value) {
        if (value < 0) {
            throw new Error("Count cannot be negative");
        }
    }

    // Public methods:
    increment() {
        this.#count++;
    }

    decrement() {
        this.#validateCount(this.#count - 1);
        this.#count--;
    }

    // getter
    get count() {
        return this.#count;
    }
}

// Example usage:
const myCounter = new Counter();

myCounter.increment();
myCounter.increment();
console.log(myCounter.count); // Output: 2

myCounter.decrement();
console.log(myCounter.count); // Output: 1

console.log(myCounter.#count); // SyntaxError: Private field '#count'
console.log(myCounter.#validateCount(5)); // SyntaxError: Private field '#validateCount'

```
