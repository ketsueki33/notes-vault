---
difficulty: medium
leetcode-num: 241
topics:
  - Math
  - String
  - Dynamic Programming
  - Recursion
---
[Problem Link](https://leetcode.com/problems/different-ways-to-add-parentheses/)

#### Problem
Given a string `expression` of numbers and operators, return _all possible results from computing all the different possible ways to group numbers and operators_. You may return the answer in **any order**.

The test cases are generated such that the output values fit in a 32-bit integer and the number of different results does not exceed `104`.

**Example 1:**

**Input:** expression = `"2-1-1"`
**Output:** `[0,2]`
**Explanation:**
`((2-1)-1) = 0 `
`(2-(1-1)) = 2`

**Example 2:**

**Input:** expression = `"2*3-4*5"`
**Output:** `[-34,-14,-10,-10,10]`
**Explanation:**
`(2*(3-(4*5))) = -34 `
`((2*3)-(4*5)) = -14 `
`((2*(3-4))*5) = -10 `
`(2*((3-4)*5)) = -10 `
`(((2*3)-4)*5) = 10`

**Constraints:**

- `1 <= expression.length <= 20`
- `expression` consists of digits and the operator `'+'`, `'-'`, and `'*'`.
- All the integer values in the input expression are in the range `[0, 99]`.
- The integer values in the input expression do not have a leading `'-'` or `'+'` denoting the sign.

#### Solution
[Video Explanation](https://youtu.be/vWW67t_a--8)

##### Recursive with Memoization- DP Approach
We use a dp matrix to memoize the solution of each recursive call.

*TC ->* O(  n$^{3}$  )
*SC ->* O(  n$^{2}$   )

```cpp title=Code
class Solution {
    vector<vector<vector<int>>> dp;

    vector<int> solve(string& s, int start, int end) {
        if (!dp[start][end].empty())
            return dp[start][end];

        vector<int> res;

        for (int i = start; i <= end; i++) {
            char ch = s[i];
            if (ch == '-' || ch == '+' || ch == '*') {

                vector<int> left = solve(s, start, i - 1);
                vector<int> right = solve(s, i + 1, end);

                for (int& x : left) {
                    for (int& y : right) {
                        if (ch == '-') {
                            res.push_back(x - y);
                        } else if (ch == '+') {
                            res.push_back(x + y);
                        } else if (ch == '*') {
                            res.push_back(x * y);
                        }
                    }
                }
            }
        }

        if (res.size() == 0)
            res.push_back(stoi(s.substr(start, end - start + 1)));

        dp[start][end] = res;
        return dp[start][end];
    }

public:
    vector<int> diffWaysToCompute(string expression) {
        dp.resize(expression.size(),
                  vector<vector<int>>(expression.size(), vector<int>(0)));
        return solve(expression, 0, expression.size() - 1);
    }
};
```

##### Pure Recursive Approach

*TC ->* O( n * 2$^{n}$  )
*SC ->* O( n * 2$^{n}$   )

```cpp title=Code
class Solution {

    vector<int> solve(string& s, int start, int end) {
        vector<int> res;

        for (int i = start; i <= end; i++) {
            char ch = s[i];
            if (ch == '-' || ch == '+' || ch == '*') {

                vector<int> left = solve(s, start, i - 1);
                vector<int> right = solve(s, i + 1, end);

                for (int& x : left) {
                    for (int& y : right) {
                        if (ch == '-') {
                            res.push_back(x - y);
                        } else if (ch == '+') {
                            res.push_back(x + y);
                        } else if (ch == '*') {
                            res.push_back(x * y);
                        }
                    }
                }
            }
        }

        if (res.size() == 0)
            res.push_back(stoi(s.substr(start, end - start + 1)));

        return res;
    }

public:
    vector<int> diffWaysToCompute(string expression) {
        return solve(expression, 0, expression.size() - 1);
    }
};
```