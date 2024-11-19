---
difficulty: medium
leetcode-num: 1937
topics:
  - Array
  - Dynamic Programming
---
[Problem Link](https://leetcode.com/problems/maximum-number-of-points-with-cost/)

#### Problem
You are given an `m x n` integer matrix `points` (**0-indexed**). Starting with `0` points, you want to **maximize** the number of points you can get from the matrix.

To gain points, you must pick one cell in **each row**. Picking the cell at coordinates `(r, c)` will **add** `points[r][c]` to your score.

However, you will lose points if you pick a cell too far from the cell that you picked in the previous row. For every two adjacent rows `r` and `r + 1` (where `0 <= r < m - 1`), picking cells at coordinates `(r, c1)` and `(r + 1, c2)` will **subtract** `abs(c1 - c2)` from your score.

Return _the **maximum** number of points you can achieve_.

`abs(x)` is defined as:

- `x` for `x >= 0`.
- `-x` for `x < 0`.

**Example 1:**

![|200](https://assets.leetcode.com/uploads/2021/07/12/screenshot-2021-07-12-at-13-40-26-diagram-drawio-diagrams-net.png)

**Input:** points = `[[1,2,3],[1,5,1],[3,1,1]]`
**Output:** 9
**Explanation:**
The blue cells denote the optimal cells to pick, which have coordinates (0, 2), (1, 1), and (2, 0).
You add 3 + 5 + 3 = 11 to your score.
However, you must subtract abs(2 - 1) + abs(1 - 0) = 2 from your score.
Your final score is 11 - 2 = 9.

**Example 2:**

![|150](https://assets.leetcode.com/uploads/2021/07/12/screenshot-2021-07-12-at-13-42-14-diagram-drawio-diagrams-net.png)

**Input:** points = `[[1,5],[2,3],[4,2]]`
**Output:** 11
**Explanation:**
The blue cells denote the optimal cells to pick, which have coordinates (0, 1), (1, 1), and (2, 0).
You add 5 + 3 + 4 = 12 to your score.
However, you must subtract abs(1 - 1) + abs(1 - 0) = 1 from your score.
Your final score is 12 - 1 = 11.

**Constraints:**

- `m == points.length`
- `n == points[r].length`
- `1 <= m, n <= 105`
- `1 <= m * n <= 105`
- `0 <= points[r][c] <= 105`

#### Solution

Note:- Solution can be improved by maintaining a `vector<long>` instead of duplicating the input matrix in `long` data-type.

*TC ->* O(  )
*SC ->* O(  )

```cpp title=Code
class Solution {
    vector<vector<long long>> dp;

public:
    long long maxPoints(vector<vector<int>>& points) {
        for (int i = 0; i < points.size(); i++) {
            vector<long long> temp;
            for (int j = 0; j < points[0].size(); j++)
                temp.push_back((long long)points[i][j]);
            dp.push_back(temp);
        }

        for (int r = 0; r < dp.size() - 1; r++) {
            vector<long long> temp = dp[r];
            long long left = -1, right = -1;
            for (int i = 0; i < temp.size(); i++) {
                left--;
                temp[i] = max(left, temp[i]);
                left = temp[i];
            }
            for (int i = temp.size() - 1; i >= 0; i--) {
                right--;
                temp[i] = max(right, temp[i]);
                right = temp[i];
            }
            vector<long long>& next = dp[r + 1];
            for (int i = 0; i < temp.size(); i++) {
                next[i] += temp[i];
            }
        }
        vector<long long>& v = dp.back();
        return *max_element(v.begin(), v.end());
    }
};
```