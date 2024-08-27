In JavaScript, a closure is a powerful feature where an inner function has access to the outer (enclosing) function's variables and parameters, even after the outer function has finished executing. Closures are created whenever a function is created within another function, and they allow for the encapsulation of state and functionality.

**Example)**
```js
function outerFunction() {
    let outerVariable = 'I am outside!';

    function innerFunction() {
        console.log(outerVariable); // The inner function has access to the outer function's variable
    }

    return innerFunction;
}

const myFunction = outerFunction();
myFunction(); // Output: I am outside!
```

#### More Examples

**Ex) Create a closure that maintains a private counter. Implement functions to increment and get current value**
```js
const outerFunction = () => {
    let private = 0;

    const show = () => {
        console.log(private);
    };

    const increment = () => {
        private++;
        console.log("1UP");
    };

    return [show, increment];
};

const [f1, f2] = outerFunction();

f1();// 0
f2();// 1UP
f1();// 1
f2();// 1UP
f2();// 1UP
f1();// 3
```

**Ex) Create a closure that captures a user's name and returns a function that greets the user by name**
```js
const createGreeting = (name)=>{
    return ()=>`Hello ${name}.. Welcome to JavaScript`;
}

const leclerc = createGreeting("Charles");
const piastri = createGreeting("Oscar");

console.log(leclerc()); // Hello Charles.. Welcome to JavaScript
console.log(piastri()); // Hello Oscar.. Welcome to JavaScript
```

**Ex) Write a loop that creates an array of functions. Each functions should log its index when called.**
```js
const arrayOfFunctions = (n) => {
    const arr = [];
    for (let i = 0; i < n; i++) {
        arr.push(() => console.log(i));
    }

    return arr;
};

const nFunctions = arrayOfFunctions(5);

for (let i = 0; i < 5; i++) {
    nFunctions[[|i]];
}
//Output:
// 0
// 1
// 2
// 3
// 4
```

#### Module Pattern
The module pattern in JavaScript is a design pattern used to create encapsulated and reusable code. It leverages closures to provide a way to create private variables and functions that cannot be accessed from the outside, while exposing only the methods and variables that should be public.The module pattern in JavaScript is a design pattern used to create encapsulated and reusable code. It leverages closures to provide a way to create private variables and functions that cannot be accessed from the outside, while exposing only the methods and variables that should be public.

**Advantages of the Module Pattern:**
1. **Encapsulation**: Keeps the module's internal state private.
2. **Public API**: Clearly defines what is exposed publicly.
3. **Maintainability**: Easier to maintain and understand code due to clear separation of public and private parts.
4. **Namespace**: Avoids polluting the global namespace, reducing the risk of naming conflicts.

```js
const myModule = (function() {
    // Private variables and functions
    let privateVariable = 'I am private';
    
    function privateFunction() {
        console.log(privateVariable);
    }

    // Public API
    return {
        publicMethod: function() {
            console.log('I am public');
            privateFunction();
        },
        setPrivateVariable: function(value) {
            privateVariable = value;
        }
    };
})();

// Usage
myModule.publicMethod(); // Output: I am public
                         //         I am private
myModule.setPrivateVariable('New value');
myModule.publicMethod(); // Output: I am public
                         //         New value
```

#### Memoization
Memoization is an optimization technique that involves storing the results of expensive function calls and reusing those results when the same inputs occur again. Closures can be used in JavaScript to implement memoization by encapsulating the cache within a function's scope, ensuring it is not accessible from the outside.

**Example of memoization using closures to cache the results of a factorial function:**
```js
const memoizedFactorial = () => {
    const cache = {};

    function factorial(n) {
        console.log(cache);
        if (n === 0) return 1;
        if (cache[n]) {
            return cache[n];
        }

        const result = n * factorial(n - 1);
        cache[n] = result;
        return result;
    }

    return factorial;
};

const memo = memoizedFactorial();
console.log(memo(2));
console.log(memo(3));
console.log(memo(5));
//Output:
// {}
// {}
// {}
// 2
// { '1': 1, '2': 2 }
// { '1': 1, '2': 2 }
// 6
// { '1': 1, '2': 2, '3': 6 }
// { '1': 1, '2': 2, '3': 6 }
// { '1': 1, '2': 2, '3': 6 }
// 120
```