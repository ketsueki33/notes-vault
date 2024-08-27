The `Array` object, as with arrays in other programming languages, enables storing a collection of multiple items under a single variable name, and has members for performing common array operations. 

#### Creating an Array
##### using the assignment operator
```js
const books = ["The Great Gatsby", "War and Peace", "Hamlet", "Moby Dick"];

console.log(books);
//['The Great Gatsby', 'War and Peace', 'Hamlet', 'Moby Dick']
```

##### using the new operator and Array constructor

Here is the basic syntax:

```js
new Array();
```

If a number parameter is passed into the parenthesis, that will set the length for the new array.

In this example, we are creating an array with a length of 3 empty slots.

```js
new Array(3)
```

If we use the length property on the new array, then it will return the number 3.

```js
new Array(3).length
```

But if we try to access any elements of the array, it will come back undefined because all of those slots are currently empty.

```js
new Array(3)[0]
// undefined
```

We can modify our example to take in multiple parameters and create an array of food items.

```js
let myFavoriteFoods = new Array("pizza", "ice cream", "salad");

console.log(myFavoriteFoods); // ["pizza","ice cream","salad"]
console.log(myFavoriteFoods.length); // 3
console.log(myFavoriteFoods[1]); // "ice cream"
```

##### using `Array.of()`
Another way to create an array is to use the `Array.of()` method. This method takes in any number of arguments and creates a new array instance.

Here is the basic syntax:

```js
Array.of(); 
```

We can modify our earlier food example to use the `Array.of()` method like this.

```js
let myFavoriteFoods = Array.of("pizza", "ice cream", "salad");

console.log(myFavoriteFoods); // ["pizza","ice cream","salad"]
console.log(myFavoriteFoods.length); // 3
console.log(myFavoriteFoods[1]); // "ice cream"
```

This method is really similar to using the Array constructor. The key difference is that if you pass in a single number using  `Array.of()` it will return an array with that number in it. But the Array constructor creates x number of empty slots for that number.

In this example it would return an array with the number 4 in it.
[empty × 4]
```js
let myArr = Array.of(4);

console.log(myArr); // [4]
```

But if I changed this example to use the Array constructor, then it would return an array of 4 empty slots.
```js
let myArr = new Array(4);

console.log(myArr); // [empty × 4]
```


#### Array Methods
[[#push( )]]  -  [[#pop( )]]  -  [[#shift( )]]  -  [[#unshift( )]]  -  [[#map( )]]   -  [[#filter( )]]  -  [[#reduce( )]]
[[#sort( )]]  -  [[#forEach( )]]  -   [[#concat( )]]  -  [[#every( )]]  -   [[#some( )]]  -   [[#includes( )]]   -  [[#join( )]]  -  [[#find( )]]    
[[#findIndex( )]]  -  [[#fill( )]]    -  [[#slice( )]]   -   [[#reverse( )]]    
##### Main Array Methods
###### push( )
This method adds one or more elements to the end of array and returns the new length of the array.
```js
const fruits = ["Apple", "Peach"];
console.log(fruits.push("Banana", "Orange")); //4
console.log(fruits);// [ 'Apple', 'Peach', 'Banana', 'Orange' ]
```

###### pop( )
This method removes the last element from the end of array and returns that element.
```js
const fruits = ["Apple", "Peach", "Watermelon"];
console.log(fruits.pop()); // Watermelon
console.log(fruits); // [ 'Apple', 'Peach' ]
```

###### shift( )
This method removes the first element from an array and returns that element.
```js
const fruits = ["Apple", "Peach", "Watermelon"];
console.log(fruits.shift()); // Apple
console.log(fruits); // [ 'Peach', 'Watermelon' ]
```

###### unshift( )
This method adds one or more elements to the beginning of an array and returns the new length of the array.
```js
const fruits = ["Apple", "Peach", "Watermelon"];
console.log(fruits.unshift("Jackfruit", "Papaya")); // 5
console.log(fruits); // [ 'Jackfruit', 'Papaya', 'Apple', 'Peach', 'Watermelon' ]
```

###### map( )
This method creates a new array with the results of calling a provided function on every element in this array.
```js
const arr = [1, 2, 3, 4, 5, 6];
const doubled = arr.map((element) => element * 2);
console.log(doubled); // [ 2, 4, 6, 8, 10, 12 ]
```

###### filter( )
This method creates a new array with only elements that passes the condition inside the provided function.
```js
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const onlyEven = arr.filter((element) => element % 2 === 0);
console.log(onlyEven); // [ 2, 4, 6, 8, 10 ]
```

###### reduce( )
This method applies a function against an accumulator and each element in the array to reduce it to a single value.

The `reduce` function in JavaScript determines the initial value of the accumulator based on whether an initial value is explicitly provided and the number of elements in the array. Here's how it works:

If an initial value is provided as the second argument to `reduce`:
- The accumulator starts with that value.
- The reduction begins with the first element of the array.
```js
const arr = [1, 2, 3, 4];
const sumOfAll = arr.reduce((accumulator, element) => accumulator + element, 10);
console.log(sumOfAll); // 20
```

 If no initial value is provided :
- The first element of the array becomes the initial value of the accumulator.
- The reduction begins with the second element of the array.
```js
const arr = [1, 2, 3, 4];
const sumOfAll = arr.reduce((accumulator, element) => accumulator + element);
console.log(sumOfAll); // 10
```

##### Other Array Methods
###### sort( )
This method is used to arrange/sort array’s elements either in ascending or descending order. The `sort()` method modifies the original array.

**By default, the `sort()` method sorts elements as strings in lexicographical order.**

Example of default behavior:
```js
let numbers = [1, 10, 2, 21, 3];
numbers.sort();
console.log(numbers); // Outputs: [1, 10, 2, 21, 3]
```

To sort numbers correctly, you need to provide a comparison function. The comparison function should be defined keeping these points in mind:
- If it returns a negative value, `a` is sorted before `b`.
- If it returns 0, the order of `a` and `b` remains unchanged.
- If it returns a positive value, `b` is sorted before `a`.
```js
const arr = [2, 4, -1, 100, 20, -25, 90];

const ascending = arr.sort((a, b) => (a < b ? -1 : 1));
console.log(ascending); // [-25, -1, 2, 4, 20, 90, 100]

const descending = arr.sort((a, b) => (a < b ? 1 : -1));
console.log(descending); // [100, 90,  20, 4, 2, -1, -25]
```

###### forEach( )
This method helps to loop over array by executing a provided callback function for each element in an array.
```js
const arr = [1, 2, 3, 4, 5];

arr.forEach((element, index) => {
    console.log(`Element at index ${index} is ${element}`);
});
// Element at index 0 is 1
// Element at index 0 is 1
// Element at index 1 is 2
// Element at index 2 is 3
// Element at index 3 is 4
// Element at index 4 is 5
```

###### concat( )
This method is used to merge two or more arrays and returns a new array, without changing the existing arrays.
```js
const array1 = [1, 2, 3];
const array2 = [4, 5, 6];
const array3 = [7, 8, 9];

const combinedArray = array1.concat(array2, array3);

console.log(array1); // Output: [1, 2, 3]

console.log(array2); // Output: [4, 5, 6]

console.log(array3); // Output: [7, 8, 9]

console.log(combinedArray); // Output: [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

You can also use this method to concatenate values directly.
```js
const newArray = array1.concat(4, 5, array3);
console.log(newArray); // Output: [1, 2, 3, 4, 5, 7, 8, 9]
```

###### every( )
The `every()` method tests whether all elements in the array pass the test implemented by the provided function. It returns a Boolean value.
```js
const numbers = [2, 4, 6, 8, 10];

const allEven = numbers.every((num) => num % 2 === 0);
console.log(allEven); // Output: true

const allGreaterThanFive = numbers.every((num) => num > 5);
console.log(allGreaterThanFive); // Output: false
```

If the array is empty, the `every()` method returns `true` for any condition.

###### some( )
The `some()` method tests whether at least one element in the array passes the test implemented by the provided function. It returns a Boolean value.
```js
const numbers = [1, 2, 3, 4, 5];

const hasEven = numbers.some((num) => num % 2 === 0);
console.log(hasEven); // Output: true

const hasNegative = numbers.some((num) => num < 0);
console.log(hasNegative); // Output: false
```

If the array is empty, the `some()` method returns `false` for any condition.

###### includes( )
The `includes()` method in JavaScript determines whether an array includes a certain value among its entries, returning `true` or `false` as appropriate.

```js
// Syntax:
array.includes(valueToFind, fromIndex)
```
- `valueToFind`: The value to search for in the array.
- `fromIndex` (optional): The position in the array at which to begin the search. Defaults to `0`.
```js
const array = [1, 2, 3, 4, 5];

console.log(array.includes(3)); // true
console.log(array.includes(6)); // false
console.log(array.includes(3, 3)); // false (starts searching from index 3, which is the 4th element)
console.log(array.includes(5, -1)); // true (negative index counts from the end, so -1 is the last element)
```

###### join( )
The `join()` method in JavaScript creates and returns a new string by concatenating all of the elements in an array, separated by commas or a specified separator string.
```js
const array1 = ['Fire', 'Air', 'Water'];

console.log(array1.join()); // "Fire,Air,Water"
console.log(array1.join('')); // "FireAirWater"
console.log(array1.join('-')); // "Fire-Air-Water"
console.log(array1.join(' and ')); // "Fire and Air and Water"
```

###### find( )
The `find()` method in JavaScript returns the value of the first element in an array that satisfies the provided testing function. If no elements satisfy the testing function, `undefined` is returned.
```js
const array3 = ['apple', 'banana', 'cherry'];

const found3 = array3.find(fruit => fruit === 'banana');
console.log(found3); // "banana"

const found4 = array3.find(fruit => fruit === 'grape');
console.log(found4); // undefined
```

###### findIndex( )
The `findIndex()` method in JavaScript returns the index of the first element in an array that satisfies the provided testing function. If no elements satisfy the testing function, it returns `-1`.
```js
const array3 = ['apple', 'banana', 'cherry'];

const index3 = array3.findIndex(fruit => fruit === 'banana');
console.log(index3); // 1

const index4 = array3.findIndex(fruit => fruit === 'grape');
console.log(index4); // -1
```

###### fill( )
The `fill()` method in JavaScript fills all the elements of an array from a start index to an end index with a static value. The end index is not included. If no start and end indices are provided, the method fills the entire array.
```js
const array1 = [1, 2, 3, 4, 5];

// Fill all elements with 0
console.log(array1.fill(0)); // [0, 0, 0, 0, 0]

// Fill elements from index 2 to end with 6
console.log(array1.fill(6, 2)); // [0, 0, 6, 6, 6]

const array2 = [1, 2, 3, 4, 5];

// Fill elements from index 1 to index 3 with 7
console.log(array2.fill(7, 1, 3)); // [1, 7, 7, 4, 5]

// Fill elements from index -3 to end with 9
console.log(array2.fill(9, -3)); // [1, 7, 7, 9, 9]
```

###### slice( )
The `slice()` method in JavaScript returns a shallow copy of a portion of an array into a new array object selected from start to end (end not included). The original array will not be modified.
```js
const array1 = ['a', 'b', 'c', 'd', 'e'];

// Slice from index 2 to the end
const slice1 = array1.slice(2);
console.log(slice1); // ['c', 'd', 'e']

// Slice from index 1 to 3 (not including 3)
const slice2 = array1.slice(1, 3);
console.log(slice2); // ['b', 'c']

// Slice the last two elements
const slice3 = array1.slice(-2);
console.log(slice3); // ['d', 'e']

// Slice from index -4 to -1 (not including -1)
const slice4 = array1.slice(-4, -1);
console.log(slice4); // ['b', 'c', 'd']

// Slice the entire array
const slice5 = array1.slice();
console.log(slice5); // ['a', 'b', 'c', 'd', 'e']
```

###### reverse( )
The `reverse()` method in JavaScript reverses an array in place. The first array element becomes the last, and the last array element becomes the first. This method modifies the original array and also returns the reversed array.
```js
const array1 = ['a', 'b', 'c', 'd', 'e'];

const reversedArray1 = array1.reverse();
console.log(reversedArray1); // ['e', 'd', 'c', 'b', 'a']
console.log(array1); // ['e', 'd', 'c', 'b', 'a'] (original array is modified)
```


#### Array Iteration
##### for loop
[[Loops#For Loop]]
```js
const fruits = ["Apple", "Peach", "Watermelon", "Jackfruit", "Papaya"];

for (let i = 0; i < fruits.length; i++) 
    console.log(fruits[i]);
// Apple
// Peach
// Watermelon
// Jackfruit
// Papaya

for (let fruit of fruits) {
    console.log(fruit);
}

for (let index in fruits) {
    console.log(fruits[index]);
}
```


##### forEach Method
This method helps to loop over array by executing a provided callback function for each element in an array.
```js
const arr = [1, 2, 3, 4, 5];

arr.forEach((element, index) => {
    console.log(`Element at index ${index} is ${element}`);
});
// Element at index 0 is 1
// Element at index 0 is 1
// Element at index 1 is 2
// Element at index 2 is 3
// Element at index 3 is 4
// Element at index 4 is 5
```

#### Multi-dimensional Arrays
A multidimensional array in JavaScript is an array that has one or more nested arrays. A normal array, we know, is a one-dimensional array that can be accessed using a single index, `arr[i]` where as a two-dimensional array can be accessed using two indices, `arr[i][j]`, Similarly for three-dimensional array `arr[i][j][k]` and so on.

```js
// Create a 2D array
const matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

// Log the entire matrix
for (let i = 0; i < matrix.length; i++) {
    console.log(matrix[i]);
}
// [ 1, 2, 3 ]
// [ 4, 5, 6 ]
// [ 7, 8, 9 ]

// Access a specific element
const specificElement = matrix[1][2];
console.log("Specific element at matrix[1][2]:", specificElement);
// Specific element at matrix[1][2]: 6
```