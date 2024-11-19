---
difficulty: medium
leetcode-num: 2684
topics:
  - Array
  - Dynamic Programming
  - Matrix
---
[Problem Link](https://leetcode.com/problems/maximum-number-of-moves-in-a-grid/)

#### Problem
You are given a **0-indexed** `m x n` matrix `grid` consisting of **positive** integers.

You can start at **any** cell in the first column of the matrix, and traverse the grid in the following way:

- From a cell `(row, col)`, you can move to any of the cells: `(row - 1, col + 1)`, `(row, col + 1)` and `(row + 1, col + 1)` such that the value of the cell you move to, should be **strictly** bigger than the value of the current cell.

Return _the **maximum** number of **moves** that you can perform._

**Example 1:**

![|200](https://assets.leetcode.com/uploads/2023/04/11/yetgriddrawio-10.png)

**Input:** `grid = [[2,4,3,5],[5,4,9,3],[3,4,2,11],[10,9,13,15]]`
**Output:** 3
**Explanation:** We can start at the cell (0, 0) and make the following moves:
- (0, 0) -> (0, 1).
- (0, 1) -> (1, 2).
- (1, 2) -> (2, 3).
It can be shown that it is the maximum number of moves that can be made.

**Example 2:**

![|150](https://assets.leetcode.com/uploads/2023/04/12/yetgrid4drawio.png)

**Input:** `grid = [[3,2,4],[2,1,9],[1,1,7]]`
**Output:** 0
**Explanation:** Starting from any cell in the first column we cannot perform any moves.

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 1000`
- `4 <= m * n <= 105`
- `1 <= grid[i][j] <= 106`

#### Solution

##### DP Memoization Recursion - Optimal Approach
###### 1. Key Variables and Setup

- `rows` and `cols`: Store grid dimensions for easy access.
- `dp`: A 2D vector for memoization, where each cell `dp[row][col]` will store the max moves possible starting from that cell. Initially set to `-1` to indicate cells that haven’t been computed.
- `dRow`: An array representing the row direction offsets to explore neighbors (top, middle, and bottom of the next column).

###### 2. Helper Function `isSafe()`

- **Purpose**: Checks if moving from the current cell `(row, col)` to a neighbor `(nRow, nCol)` is within bounds and satisfies the "move condition" (i.e., the next cell’s value must be greater than the current cell’s).
- **Parameters**:
    - `nRow` and `nCol`: The potential next row and column positions.
    - `curr`: The current cell value.
    - `grid`: The grid matrix.
- **Return Value**: Returns `true` if the move is valid, otherwise `false`.

###### 3. Recursive Function `solve()`

- **Purpose**: Computes the max moves starting from a given cell `(row, col)`.
- **Base Cases**:
    - If `dp[row][col]` is already computed (`dp[row][col] != -1`), return its value (memoization).
    - If we're in the last column, return 0 (since we can't move further).
- **Recursive Logic**:
    - Iterate over the three possible row moves (`dRow`), calculate `nRow` (next row), and use `col + 1` as the next column.
    - For each valid neighbor, recursively calculate `solve(nRow, col + 1)` and store results in vector `v`.
    - Compute `dp[row][col]` as `1 + max(v)`, which includes the move to the next column plus the max moves from that point onward.

###### 4. Main Function `maxMoves()`

- **Purpose**: Initializes the `dp` table and starts exploring from each row in the first column.
- Loops through each row `i` in the first column (`col = 0`), calling `solve(grid, i, 0)`.
- Tracks the maximum moves across all possible starting points in the first column.

*TC ->* O( `m * n` )
*SC ->* O( `m * n` )

```cpp title=Code
class Solution {
    int rows, cols;
    vector<vector<int>> dp;

    int dRow[3] = {-1, 0, +1};

    bool isSafe(int nRow, int nCol, int curr, vector<vector<int>>& grid) {
        if (nRow < 0 || nCol < 0 || nRow >= rows || nCol >= cols)
            return false;

        if (curr >= grid[nRow][nCol])
            return false;

                return true;
    }

    int solve(vector<vector<int>>& grid, int row, int col) {
        if (dp[row][col] != -1)
            return dp[row][col];

        if (col == cols - 1)
            return dp[row][col] = 0;

        vector<int> v = {-1};

        for (int i = 0; i < 3; i++) {
            int nRow = row + dRow[i];

            if (!isSafe(nRow, col + 1, grid[row][col], grid))
                continue;

            v.push_back(solve(grid, nRow, col + 1));
        }

        return dp[row][col] = *max_element(v.begin(), v.end()) + 1;
    }

public:
    int maxMoves(vector<vector<int>>& grid) {
        rows = grid.size();
        cols = grid[0].size();
        int res = 0;

        dp.resize(rows, vector<int>(cols, -1));

        for( int i= 0 ; i < rows ; i++ )
            res = max(res,solve(grid,i,0));

        return res;
    }
};
```