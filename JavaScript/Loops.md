A loop is a programming tool that is used to repeat a set of instructions.

#### For Loop
Ex) Write a program to print numbers from 1 to 10.
```js
const print1to10 = () => {
    for (let i = 1; i <= 10; i++) {
        console.log(i);
    }
};
```
**Other variations:**
```ts
const fruits = ["Apple", "Peach", "Watermelon", "Jackfruit", "Papaya"];

for (let fruit of fruits) {
    console.log(fruit);
}

for (let index in fruits) {
    console.log(fruits[index]);
}
```
#### While Loop
Ex) Write a program to print numbers from 10 to 1.
```js
const print10to1 = () => {
    let i = 10;
    while (i >= 0) {
        console.log(i);
        i--;
    }
};
```

#### Do-While Loop
Ex) Write a program to find the factorial of a number.
```js
const factorial = (x) => {
    if (x < 0) return -1;
    let ans = 1;
    do {
        ans = ans * x;
        x--;
    } while (x > 1);

    return ans;
};
```

#### Nested Loops
Ex) Print the following pattern:

![[Pasted image 20240717121347.png|150]]

```js
const pattern = () => {
    for (let i = 1; i <= 5; i++) {
        let str = "";

        for (let j = 1; j <= i; j++)
            str += "* ";
        
        console.log(str);
    }
};
```

#### Loop Control Statements
##### break statement
The **`break`** statement terminates the current loop or [[Control Structures#Switch Case|switch]] statement and transfers program control to the statement following the terminated statement.

Ex) Print 1 to 10 but stop when 7 is reached.
```js
const stopAt7 = () =>{
    for (let i = 1; i <= 10; i++) {
        if( i === 7 )
            break;
        console.log(i);
    }
}
```

##### continue statement
The **`continue`** statement terminates execution of the statements in the current iteration of the current or labeled loop, and continues execution of the loop with the next iteration.

Ex) Print 1 to 10 but skip 5.
```js
const skip5 = () => {
    for (let i = 1; i <= 10; i++) {
        if( i === 5 )
            continue;
        console.log(i);
    }
}
```