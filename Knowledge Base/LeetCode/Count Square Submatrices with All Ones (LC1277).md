---
difficulty: medium
leetcode-num: 1277
topics:
  - Array
  - Matrix
  - Dynamic Programming
---
[Problem Link](https://leetcode.com/problems/count-square-submatrices-with-all-ones/)

#### Problem
Given a `m * n` matrix of ones and zeros, return how many **square** submatrices have all ones.

**Example 1:**

**Input:** matrix =
[
  [0,1,1,1],
  [1,1,1,1],
  [0,1,1,1]
]
**Output:** 15
**Explanation:** 
There are **10** squares of side 1.
There are **4** squares of side 2.
There is  **1** square of side 3.
Total number of squares = 10 + 4 + 1 = **15**.

**Example 2:**

**Input:** matrix = 
[
  [1,0,1],
  [1,1,0],
  [1,1,0]
]
**Output:** 7
**Explanation:** 
There are **6** squares of side 1.  
There is **1** square of side 2. 
Total number of squares = 6 + 1 = **7**.

**Constraints:**

- `1 <= arr.length <= 300`
- `1 <= arr[0].length <= 300`
- `0 <= arr[i][j] <= 1`

#### Solution
[Video Explanation](https://youtu.be/y3kdowdyNMM)

##### DP Tabulation - Approach
- **Initialize Matrix Dimensions and `dp` Table**:
    
    - `m` and `n` store the dimensions of the matrix.
    - `dp` is a `2D` vector of size `(m+1) x (n+1)` initialized to `0`. The extra row and column allow for easier boundary checks.
- **Looping Through Matrix (Bottom-Up)**:
    
    - Start from the bottom-right of the matrix and work backward to the top-left. For each cell `(i, j)`:
        - **Skip Cells with 0**: If `matrix[i][j]` is `0`, no square submatrix can end at this cell, so we skip to the next cell.
        - **Calculate `dp[i][j]` for Cells with 1**:
            - If `matrix[i][j]` is `1`, the maximum square submatrix ending at this cell depends on three neighboring cells:
                - `dp[i+1][j]`: cell directly below.
                - `dp[i][j+1]`: cell directly to the right.
                - `dp[i+1][j+1]`: diagonal cell.
            - The side length of the largest square ending at `(i, j)` is `1 + min(dp[i+1][j], dp[i][j+1], dp[i+1][j+1])`.
            - This value is stored in `dp[i][j]`.
- **Updating Result**:
    
    - Each `dp[i][j]` value represents the size of the largest square ending at that cell. Adding `dp[i][j]` to `res` includes all squares of size `1` through the size represented by `dp[i][j]`.
- **Final Return Value**:
    
    - The variable `res` contains the total count of all square submatrices filled with `1`s.

*TC ->* O( `m * n` )
*SC ->* O( `m * n` )

```cpp title=Code
class Solution {

public:
    int countSquares(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        vector<vector<int>> dp;
        dp.resize(m + 1, vector<int>(n + 1, 0));

        int res = 0;

        // dp[i][j] = total square submatrices (having 1s) starting at
        // cell[i][j]
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                if (matrix[i][j] != 1)
                    continue;

                dp[i][j] =
                    1 + min({dp[i + 1][j], dp[i + 1][j + 1], dp[i][j + 1]});

                res += dp[i][j];
            }
        }
        return res;
    }
};
```

##### DP Memoization - Optimal Approach
1. **Initialize Variables**:
    
    - `m` and `n` store the matrix's dimensions.
    - `dp` is a 2D vector initialized to `-1` for each cell, used for memoization to store subproblem results (squares ending at each cell).
2. **Recursive Helper Function: `solve(int i, int j, vector<vector<int>>& matrix)`**:
    
    - This function calculates the number of the total squares starting at cell `(i, j)` and returns that.
    - **Base Case**:
        - If `i` or `j` goes out of bounds (`i == m` or `j == n`), we return `0` because we're outside the matrix.
    - **Memoization Check**:
        - If `dp[i][j]` is not `-1`, we’ve already computed the value for this cell, so we return it to save computation.
    - **Cell Check**:
        - If `matrix[i][j]` is `0`, no square can end at this cell, so `dp[i][j]` is set to `0` and returned immediately.
    - **Recursive Calls**:
        - If `matrix[i][j]` is `1`, we look at three adjacent cells to determine the maximum square starting at `(i, j)`:
            - `right`: the square size starting at the cell to the right `(i, j + 1)`.
            - `bottom`: the square size starting at the cell below `(i + 1, j)`.
            - `bottomRight`: the square size starting at the diagonal cell `(i + 1, j + 1)`.
        - The largest square starting at `(i, j)` will have a side length of `1 + min({right, bottom, bottomRight})`.
            - This is stored in `dp[i][j]` to avoid recalculating it later.
3. **Counting Squares in `countSquares()`**:
    
    - This function initializes `m`, `n`, and `dp`, and then iterates through each cell in the matrix.
    - For each cell `(i, j)`, it calls `solve(i, j, matrix)` to calculate the total squares starting at `(i, j)`, and adds it to the result `res`.


###### Key Points

- **Memoization**: `dp[i][j]` stores the maximum side length of a square ending at `(i, j)`, so we don’t recalculate it.
- **Recursive Minimization**: The square starting at `(i, j)` depends on the minimum size of squares ending at the right, bottom, and bottom-right cells.
- **Result Accumulation**: `res` accumulates the sizes of all possible squares, as each square of size `k` contributes `k` to the count.

==
*TC ->* O( `m * n` )
*SC ->* O( `m * n` )

```cpp title=Code
class Solution {
    int m, n;
    vector<vector<int>> dp;

    int solve(int i, int j, vector<vector<int>>& matrix) {
        if (i == m || j == n)
            return 0;

        if (dp[i][j] != -1)
            return dp[i][j];

        if (matrix[i][j] == 0) {
            dp[i][j] = 0;
            return 0;
        }

        int right = solve(i, j + 1, matrix);
        int bottom = solve(i + 1, j, matrix);
        int bottomRight = solve(i + 1, j + 1, matrix);

        return dp[i][j] = 1 + min({right, bottom, bottomRight});
    }

public:
    int countSquares(vector<vector<int>>& matrix) {
        m = matrix.size();
        n = matrix[0].size();

        dp.resize(m, vector<int>(n, -1));

        int res = 0;

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                res += solve(i, j, matrix);
            }
        }

        return res;
    }
};
```