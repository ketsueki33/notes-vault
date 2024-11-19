---
difficulty: medium
leetcode-num: 73
topics:
  - Array
  - Matrix
---
**[Problem Link](https://leetcode.com/problems/set-matrix-zeroes/description/)**
#### Problem 

Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.

You must do it [in place](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**

![|500](https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg)

**Input:** matrix = `[[1,1,1],[1,0,1],[1,1,1]]`
**Output:** `[[1,0,1],[0,0,0],[1,0,1]]`

**Example 2:**

![|500](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

**Input:** matrix = `[[0,1,2,0],[3,4,5,2],[1,3,1,5]]`
**Output:** `[[0,0,0,0],[0,4,5,0],[0,3,1,0]]`

**Constraints:**

- `m == matrix.length`
- `n == matrix[0].length`
- `1 <= m, n <= 200`
- `-231 <= matrix[i][j] <= 231 - 1`

#### Solution
[Video Explanation](https://youtu.be/N0MgLvceX7M)

##### Optimal Approach
Instead of using two extra arrays to mark rows and columns, we will use the first row and column of the matrix itself. But the first cell will overlap for both, so we will use an extra variable to mark the first row. 

![[Set Matrix Zeroes 1.excalidraw|600]]

**Step 1**
We will traverse through the whole matrix and if we encounter a `0`, we will mark it's corresponding cell in the first row and column as `0`. Only if we are in the first row, we will mark the `extra` variable as 0 instead of the first row. 

**Step 2**
We will traverse the part that is not used for tracking.. as interfering with the first row and column will affect the final answer. 

We will check if the corresponding row and column markers are `0`, if either is `0` we will mark the cell as `0`

![[Set Matrix Zeroes ( LC73 ) 2024-06-26 02.52.02.excalidraw|200]]

**Step 3**
We will now fill the first row and column. Remember that the marker for first row is `extra` variable and marker for first column is `matrix[0][0]`. 

> [!Important] 
> We will  fill the first column (using `matrix[0][0]`) before the first row (using `extra`) as filling the first row before may modify the value of `matrix[0][0]` which has to be used for filling the first column and will give us the wrong answer.
> 

TC -> `2 * m * n `
SC -> `m * n`

```cpp title=Code
class Solution {
   public:
    void setZeroes(vector<vector<int>>& matrix) {
        int n = matrix.size();
        int m = matrix[0].size();

        int extra_row0 = 1;

		// Step 1
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j] != 0) continue;

                if (i == 0)
                    extra_row0 = 0;
                else
                    matrix[i][0] = 0;

                matrix[0][j] = 0;
            }
        }
        
		// Step 2
        for (int i = 1; i < n; i++) {
            for (int j = 1; j < m; j++) {
                if (matrix[0][j] == 0 || matrix[i][0] == 0)
                    matrix[i][j] = 0;
            }
        }

		// Step 3
        if (matrix[0][0] == 0)
            for (int i = 0; i < n; i++)
                matrix[i][0] = 0;

        if (extra_row0 == 0)
            for (int j = 0; j < m; j++)
                matrix[0][j] = 0;
    }
};
```
##### Easy/Better Approach
We will create two new arrays to keep track of which rows and columns are to be filled with `0`. 

Then we will traverse the matrix and if either the cell's corresponding row or column is marked, then we fill set the cell to `0`. 

TC -> `2 * m * n `
SC-> `m + n `
