---
difficulty: easy
leetcode-num: 119
topics:
  - Array
  - Dynamic Programming
---
[Problem Link](https://leetcode.com/problems/pascals-triangle-ii/)

#### Problem
Given an integer `rowIndex`, return the `rowIndexth` (**0-indexed**) row of the **Pascal's triangle**.

In **Pascal's triangle**, each number is the sum of the two numbers directly above it as shown:

![|250](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

**Example 1:**

**Input:** rowIndex = 3
**Output:** `[1,3,3,1]`

**Example 2:**

**Input:** rowIndex = 0
**Output:** `[1]`

**Example 3:**

**Input:** rowIndex = 1
**Output:** `[1,1]`

**Constraints:**

- `0 <= rowIndex <= 33`
#### Solution
##### Optimal Approach
[Video Explanation](https://youtu.be/bR7mQgwQ_o8?t=655)

- We initialize the result vector `res` with the first element 1, which is always present in every row.
- We use a single variable `x` to calculate each element of the row.
- The **key insight** is that each element in a row can be calculated from the previous element using the formula: `next_element = previous_element * (row - column) / column`
- We start with `x = 1` (the first element) and iteratively calculate each subsequent element.
- In each iteration:
    - We multiply `x` by `(row - i)` and divide it by `i`.
    - This gives us the next element of the row.
    - We add this new element to our result vector.
- We use `long long` for `x` to handle potential overflow for large row indices.
- The loop runs `rowIndex` times, generating all elements of the requested row.

*TC ->* O( r ), where r is the row index
*SC ->* O( r ), where r is the row index

```cpp title=Code
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        int row = rowIndex + 1;
        vector<int> res(1, 1);
        long long x = 1;

        for (int i = 1; i < row; i++) {
            x = x * (row - i);
            x = x / i;
            res.push_back(x);
        }
        return res;
    }
};
```

#### Similar Problems
- [[Pascal's Triangle (LC118)]]
- [[Pascal's Triangle III (Finding nCr in minimal time)]]