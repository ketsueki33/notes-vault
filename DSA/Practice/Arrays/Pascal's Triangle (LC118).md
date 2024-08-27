---
difficulty: easy
leetcode-num: 118
topics:
  - Array
  - Dynamic Programming
---
[Problem Link](https://leetcode.com/problems/pascals-triangle/)

#### Problem
Given an integer `numRows`, return the first numRows of **Pascal's triangle**.

In **Pascal's triangle**, each number is the sum of the two numbers directly above it as shown:

![|250](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

**Example 1:**

**Input:** numRows = 5
**Output:** `[[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]`

**Example 2:**

**Input:** numRows = 1
**Output:** `[[1]]`

**Constraints:**

- `1 <= numRows <= 30`

#### Solution
[[|Video Explanation]]

##### Optimal Approach

The simple observation is that for each non-border element at index `i` in every row, the value is equal to sum of `previousRow[i]`+`previousRow[i-1]`. 

The first two rows will always be `[1]` & `[1,1]`.
The first and last element of each row after that is `1`.

*TC ->* O(  n$^{2}$ )
*SC ->* O( n$^{2}$ )

```cpp title=Code
class Solution {
   public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> res;
        res.pb({1});
        if (numRows == 1) return res;
        res.pb({1, 1});
        for (int i = 3; i <= numRows; i++) {
            vector<int> v(i, 1);
            vector<int> temp = res.back();
            for (int j = 1; j < i - 1; j++) {
                v[j] = temp[j] + temp[j - 1];
            }
            res.pb(v);
        }
        return res;
    }
};
```


#### Similar Problems
- [[Pascal's Triangle III (Finding nCr in minimal time)]]
- [[Pascal's Triangle II (LC119)]]
