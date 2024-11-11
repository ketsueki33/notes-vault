---
difficulty: medium
leetcode-num: 2906
topics:
  - Array
  - Matrix
  - Prefix Sum
---
[Problem Link](https://leetcode.com/problems/construct-product-matrix/)

#### Problem
Given a **0-indexed** 2D integer matrix `grid` of size `n * m`, we define a **0-indexed** 2D matrix `p` of size `n * m` as the **product** matrix of `grid` if the following condition is met:

- Each element `p[i][j]` is calculated as the product of all elements in `grid` except for the element `grid[i][j]`. This product is then taken modulo `12345`.

Return _the product matrix of_ `grid`.

**Example 1:**

**Input:** `grid = [[1,2],[3,4]]`
**Output:** `[[24,12],[8,6]]`
**Explanation:**
```
p[0][0] = grid[0][1] * grid[1][0] * grid[1][1] = 2 * 3 * 4 = 24
p[0][1] = grid[0][0] * grid[1][0] * grid[1][1] = 1 * 3 * 4 = 12
p[1][0] = grid[0][0] * grid[0][1] * grid[1][1] = 1 * 2 * 4 = 8
p[1][1] = grid[0][0] * grid[0][1] * grid[1][0] = 1 * 2 * 3 = 6
So the answer is [[24,12],[8,6]].
```

**Example 2:**

**Input:** `grid = [[12345],[2],[1]]`
**Output:** `[[2],[0],[0]]`
**Explanation:** 
```
p[0][0] = grid[0][1] * grid[0][2] = 2 * 1 = 2.
p[0][1] = grid[0][0] * grid[0][2] = 12345 * 1 = 12345. 12345 % 12345 = 0. So p[0][1] = 0.
p[0][2] = grid[0][0] * grid[0][1] = 12345 * 2 = 24690. 24690 % 12345 = 0. So p[0][2] = 0.
So the answer is [[2],[0],[0]].
```

**Constraints:**

- `1 <= n == grid.length <= 105`
- `1 <= m == grid[i].length <= 105`
- `2 <= n * m <= 105`
- `1 <= grid[i][j] <= 109`

#### Solution
[Video Explanation](https://youtu.be/kcKcDmI-VCo)

##### Optimal Approach
- **Variable Initialization**
    
    - `m` and `n`: Dimensions of the `grid`.
    - `res`: Matrix to store the final result.
    - `prefix` and `suffix`: These matrices will store cumulative products from the start (`prefix`) and end (`suffix`) of the grid.
- **Prefix Matrix Construction**
    
    - `prevProd` is initialized to 1 _outside_ the loop. As a result, the prefix matrix is calculated cumulatively across the entire grid, from top-left to bottom-right.
    - This means each element in `prefix[i][j]` holds the product of all elements in the grid up to, but not including, `grid[i][j]`, across both rows and columns.
    - After setting `prefix[i][j]` to `prevProd`, `prevProd` is updated by multiplying with `grid[i][j]` (then taken modulo `MOD`).
- **Suffix Matrix Construction**
    
    - Similarly, `prevProd` is initialized to 1 _once before_ iterating through the grid in reverse (from bottom-right to top-left).
    - `suffix[i][j]` is then assigned the cumulative product of all elements from `grid[i][j]` to the end of the grid (both rows and columns).
    - After setting `suffix[i][j]`, `prevProd` is updated with `grid[i][j]`, taken modulo `MOD`.
- **Result Matrix Construction**
    
    - For each cell `(i, j)` in the result matrix, `res[i][j]` is calculated as `(prefix[i][j] * suffix[i][j]) % MOD`.
    - This represents the product of elements before and after `grid[i][j]` in the cumulative traversal order (rather than row-by-row), effectively excluding `grid[i][j]`.

*TC ->* O( `m * n` )
*SC ->* O( `m * n` )

```cpp title=Code
class Solution {
    const int MOD = 12345;

public:
    vector<vector<int>> constructProductMatrix(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();

        vector<vector<int>> res(m, vector<int>(n));
        vector<vector<int>> prefix(m, vector<int>(n));
        vector<vector<int>> suffix(m, vector<int>(n));

        long prevProd = 1;

        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++) {
                prefix[i][j] = prevProd;
                prevProd *= grid[i][j];
                prevProd %= MOD;
            }

        prevProd = 1;

        for (int i = m - 1; i >= 0; i--)
            for (int j = n - 1; j >= 0; j--) {
                suffix[i][j] = prevProd;
                prevProd *= grid[i][j];
                prevProd %= MOD;
            }

        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                res[i][j] = (prefix[i][j] * suffix[i][j]) % MOD;

        return res;
    }
};
```