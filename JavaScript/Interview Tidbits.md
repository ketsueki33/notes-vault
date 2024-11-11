
##### Using an object as a key to access another object
```js
const a = {};
const b = { key: "b" };
const c = { key: "c" };

a[b] = "Eraser";
a[c] = "Pencil";

console.log(a[b]); // Pencil
```

In JavaScript, **object keys can only be strings or symbols**. So when we use an object as a key.. it gets converted to a string.
```js
a[b] = "Eraser";
a[c] = "Pencil";

console.log(b.toString()); // [object Object]
console.log(c.toString()); // [object Object]
```

So, when we are assigning values to the object `a`.. we are essentially doing
```js
a["[object Object]"] = "Eraser";
a["[object Object]"] = "Pencil";

console.log(a["[object Object]"]);// Pencil
```

That's why the output in Pencil.

##### let declaration in inside scope
**Q)** What will be the output of the following code:
```js
let a = 5;

{
    console.log(a);
    let a = 10;
    console.log(a);
    a = 15;
}

console.log(a);
```

**Ans)** `ReferenceError: Cannot access 'a' before initialization`
This is because when we use the same variable name `a` inside the brackets, the outside scope `a` is shadowed(forgotten) as the inside `a` declaration is hoisted to the top of the inside scope. And since we are accessing `a` in it's Temporal Dead Zone, we get a reference error.

##### block scoping to get desired output
**Q)** What will be the output of the following code:
```js
foo();

function foo() {
    console.log(1);
}

let foo = function () {
    console.log(2);
};

foo();

function foo() {
    console.log(3);
}

foo();
```
**Ans)** `SyntaxError` because we are reusing the variable name `foo` with `let`. If it was `var` , this would have been valid and the output would have been `3 2 2`. As the latest `foo` function definition is stays during creation phase (hoisting) and then that `foo` is assigned the function expression during execution phase. So the first function call before assignment will print 3 and the two after assignment will print 3.

**Follow-up Q)** Can you make this code print `1 2 3` by adding one block and changing one function declaration to function expression.

**Ans)**
```js
foo(); // Logs "1" coz of hoisting

function foo() {
    console.log(1);
}

{
    let foo = function () {
        console.log(2);
    };

    foo(); // logs "2" coz its block scoped
}

foo = function () {
    console.log(3); 
};

foo(); // logs "3" coz foo is assigned the latest function
```

##### Printing with setTimeout in a loop
**Q)** What will be the output of the following code:
```js
function x() {
    for (var i = 1; i <= 5; i++) {
        setTimeout(function () {
            console.log(i);
        }, i * 1000);
    }
}

x();
```

**Ans)** `6 6 6 6 6`. This code will log `6` five times. with intervals of 1 second. It is because the closure for each `setTimeout` callback references the variable `i`. The for loop is finished immediately and the value of `i` is 6 by the time the first callback executes.

**Follow-up Q)** Can you make it print 1 to 5?

**Ans)** 
*using `let`*
```js
function x() {
    for (let i = 1; i <= 5; i++) {
        setTimeout(function () {
            console.log(i);
        }, i * 1000);
    }
}

x();
```
- When using `let`, each iteration of the loop creates a **new instance of `i`** in the block scope, unique to that loop iteration.
- This means that in each iteration, `i` is a separate variable instance specific to that iteration’s scope.

*without using `let`*
```js
function x() {
    for (var i = 1; i <= 5; i++) {
        function closure(t) {
            setTimeout(function () {
                console.log(t);
            }, t * 1000);
        }

        closure(i);
    }
}

x();
```
- In this loop, `closure(i)` is called with `i` as the argument, passing the current value of `i` at each iteration.
- This value of `i` is passed into `closure` as the parameter `t`, which is **local to each call** of the `closure` function.

##### 'this' in object methods
**Q)** What will be the output of the following code:
```js
const obj1 = {
    value: "A",

    foo: function () {
        console.log(this.value);
    },
};

const obj2 = {
    value: "B",

    f1: obj1.foo,

    f2: function () {
        obj1.foo();
    },
    f3: () => obj1.foo(),
};

obj2.f1();
obj2.f2();
obj2.f3();
```

**Ans)** `B A A`
This is because in `f1` we are assigning the function definition of `obj1.foo`  to it. So `obj2.f1` and `obj1.foo` now reference the same function. When `obj2.f1()` is called, `this` inside `foo` points to `obj2`, because `f1` is being called directly on `obj2`. Since `obj2` has a `value` of `"B"`, the output is `"B"`.

But in `f2` and `f3`, we are calling `obj1.foo()` inside another function. For both `f1` and `f2` `this.value` in `foo` refers to `obj1.value`, which is `"A"`.