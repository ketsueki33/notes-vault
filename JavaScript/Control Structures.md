Control structures are used to _control_ the execution of a program based on a specific condition.

#### If-else
ex) program to check if a number is positive, zero or negative.
```js
const checkNum = (x) => {
    if (x == 0) return "zero";
    else if (x > 0) return "positive";
    else return "negative";
};
```

ex) program to check if a person is eligible to vote.
```js
const checkAge = (x) => {
    if (x >= 18) return "eligible to vote";
    else return "cannot vote";
};
```

#### Nested If-else
ex) program to find the largest of three numbers using nested if-else.
```js
const largestOfThree = (a, b, c) => {
    if (a > b) {
        if (a > c) return a;
        else return c;
    } else {
        if (b > c) return b;
        else return c;
    }
};
```

#### Switch Case
ex) program to log day of the week given a number.
```js
const dayOfWeek = (x) => {
    switch (x) {
        case 1:
            console.log("Monday");
            break;
        case 2:
            console.log("Tuesday");
            break;
        case 3:
            console.log("Wednesday");
            break;
        case 4:
            console.log("Thursday");
            break;
        case 5:
            console.log("Friday");
            break;
        case 6:
            console.log("Saturday");
            break;
        case 7:
            console.log("Sunday");
            break;
        default:
            console.log("invalid day");
    }
};
```

#### Conditional (Ternary) Operator
ex) program to check if a number is even or odd.
```js
const evenOrOdd = (x) => {
    return x % 2 === 0 ? "even" : "odd";
};
```

#### Combining Conditions using logical operators
ex) write a program to check if a year is leap year.
```js
const isLeap = (x) => {
    if ((x % 4 === 0 && x % 100 !== 0) || x % 400 === 0)
        return "Leap Year";
    else
        return "Not a Leap Year";
};
```