
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