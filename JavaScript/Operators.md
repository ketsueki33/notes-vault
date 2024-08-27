#### Arithmetic Operators
| Operator | Description         |
| -------- | ------------------- |
| +        | Addition            |
| -        | Subtraction         |
| *        | Multiplication      |
| **       | Exponentiation      |
| /        | Division            |
| %        | Modulus (Remainder) |
| ++       | Increment           |
| --       | Decrement           |

#### Assignment Operators
| Operator | Example | Same As    |
| -------- | ------- | ---------- |
| =        | x = y   | x = y      |
| +=       | x += y  | x = x + y  |
| -=       | x -= y  | x = x - y  |
| *=       | x *= y  | x = x * y  |
| /=       | x /= y  | x = x / y  |
| %=       | x %= y  | x = x % y  |
| **=      | x **= y | x = x ** y |
##### Shift Assignment Operators
|Operator|Example|Same As|
|---|---|---|
|<<=|x <<= y|x = x << y|
|>>=|x >>= y|x = x >> y|
|>>>=|x >>>= y|x = x >>> y|

##### Bitwise Assignment Operators
|Operator|Example|Same As|
|---|---|---|
|&=|x &= y|x = x & y|
|^=|x ^= y|x = x ^ y|
|\|=|x \|= y|x = x \| y|

#### Comparison Operators
Given that `x = 5`, the table below explains the comparison operators:

| Operator | Description                       | Comparing | Returns |
| -------- | --------------------------------- | --------- | ------- |
| ==       | equal to                          | x == 8    | false   |
|          |                                   | x == 5    | true    |
|          |                                   | x == "5"  | true    |
|          |                                   |           |         |
| ===      | equal value and equal type        | x === 5   | true    |
|          |                                   | x === "5" | false   |
|          |                                   |           |         |
| !=       | not equal                         | x != 8    | true    |
|          |                                   |           |         |
| !==      | not equal value or not equal type | x !== 5   | false   |
|          |                                   | x !== "5" | true    |
|          |                                   | x !== 8   | true    |
|          |                                   |           |         |
| >        | greater than                      | x > 8     | false   |
|          |                                   |           |         |
| <        | less than                         | x < 8     | true    |
|          |                                   |           |         |
| >=       | greater than or equal to          | x >= 8    | false   |
|          |                                   |           |         |
| <=       | less than or equal to             | x <= 8    | true    |

#### Logical Operators
Given that `x = 6` and `y = 3`, the table below explains the logical operators:

| Operator | Description | Example                     |
| -------- | ----------- | --------------------------- |
| &&       | and         | (x < 10 && y > 1) is true   |
| \|       | or          | (x == 5 \| y == 5) is false |
| !        | not         | !(x == y) is true           |

#### Ternary Operators
The **conditional (ternary) operator** is the only JavaScript operator that takes three operands: a condition followed by a question mark (`?`), then an expression to execute if the condition is [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) followed by a colon (`:`), and finally the expression to execute if the condition is [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy). This operator is frequently used as an alternative to an [`if...else`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) statement.

*syntax*:
```js
condition ? exprIfTrue : exprIfFalse
```

*example:*
```js
const age = 26;
const beverage = age >= 21 ? "Beer" : "Juice";
console.log(beverage); // "Beer"
```