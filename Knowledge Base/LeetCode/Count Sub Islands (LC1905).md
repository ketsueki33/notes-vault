---
difficulty: medium
leetcode-num: 1905
topics:
  - Array
  - Depth-First Search
  - Matrix
---
[Problem Link](https://leetcode.com/problems/count-sub-islands/)

#### Problem
You are given two `m x n` binary matrices `grid1` and `grid2` containing only `0`'s (representing water) and `1`'s (representing land). An **island** is a group of `1`'s connected **4-directionally** (horizontal or vertical). Any cells outside of the grid are considered water cells.

An island in `grid2` is considered a **sub-island** if there is an island in `grid1` that contains **all** the cells that make up **this** island in `grid2`.

Return the _**number** of islands in_ `grid2` _that are considered **sub-islands**_.

**Example 1:**

![|500](https://assets.leetcode.com/uploads/2021/06/10/test1.png)

**Input:** grid1 = `[[1,1,1,0,0],[0,1,1,1,1],[0,0,0,0,0],[1,0,0,0,0],[1,1,0,1,1]]`, grid2 = `[[1,1,1,0,0],[0,0,1,1,1],[0,1,0,0,0],[1,0,1,1,0],[0,1,0,1,0]]`
**Output:** 3
**Explanation:** In the picture above, the grid on the left is grid1 and the grid on the right is grid2.
The 1s colored red in grid2 are those considered to be part of a sub-island. There are three sub-islands.

**Example 2:**

![|500](https://assets.leetcode.com/uploads/2021/06/03/testcasex2.png)

**Input:** grid1 = `[[1,0,1,0,1],[1,1,1,1,1],[0,0,0,0,0],[1,1,1,1,1],[1,0,1,0,1]]`, grid2 = `[[0,0,0,0,0],[1,1,1,1,1],[0,1,0,1,0],[0,1,0,1,0],[1,0,0,0,1]]`
**Output:** 2 
**Explanation:** In the picture above, the grid on the left is grid1 and the grid on the right is grid2.
The 1s colored red in grid2 are those considered to be part of a sub-island. There are two sub-islands.

**Constraints:**

- `m == grid1.length == grid2.length`
- `n == grid1[i].length == grid2[i].length`
- `1 <= m, n <= 500`
- `grid1[i][j]` and `grid2[i][j]` are either `0` or `1`.

#### Solution
We will simply do a little pre-processing before we count islands in `grid2`. We will go through all the `1s` in `grid2` and check if there is a corresponding `1` in `grid1` .. if there isn't, we will simply purge the island ( mark that `1` and all connected `1s` as `0`).

After this we can just count the number of islands in `grid2` and we will get our answer.

*TC ->* O( `n * m` )
*SC ->* O(  )

```cpp title=Code
class Solution {
    int n, m;
    int dx[4] = {-1, 0, 1, 0};
    int dy[4] = {0, -1, 0, 1};

    void dfs(vector<vector<int>>& grid, int x, int y) {
        grid[x][y] = 0;

        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            if (nx >= 0 && ny >= 0 && nx < n && ny < m && grid[nx][ny] == 1) {
                dfs(grid, nx, ny);
            }
        }
    }

    int countIslands(vector<vector<int>> grid) {
        int count = 0;
        for (int i = 0; i < n; i++)
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 1) {
                    count++;
                    dfs(grid, i, j);
                }
            }
        return count;
    }

public:
    int countSubIslands(vector<vector<int>>& grid1,
                        vector<vector<int>>& grid2) {
        n = grid1.size();
        m = grid1[0].size();

        for (int i = 0; i < n; i++)
            for (int j = 0; j < m; j++) {
                if (grid2[i][j] == 1 && grid1[i][j] != 1) {
                    dfs(grid2, i, j);
                }
            }

        return countIslands(grid2);
    }
};
```