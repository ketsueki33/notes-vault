---
difficulty: medium
leetcode-num: 2326
topics:
  - Linked List
  - Matrix
  - Simulation
---
[Problem Link](https://leetcode.com/problems/spiral-matrix-iv/)

#### Problem
You are given two integers `m` and `n`, which represent the dimensions of a matrix.

You are also given the `head` of a linked list of integers.

Generate an `m x n` matrix that contains the integers in the linked list presented in **spiral** order **(clockwise)**, starting from the **top-left** of the matrix. If there are remaining empty spaces, fill them with `-1`.

Return _the generated matrix_.

**Example 1:**

![|300](https://assets.leetcode.com/uploads/2022/05/09/ex1new.jpg)

**Input:** m = 3, n = 5, head = `[3,0,2,6,8,1,7,9,4,2,5,5,0]`
**Output:** `[[3,0,2,6,8],[5,0,-1,-1,1],[5,2,4,9,7]]`
**Explanation:** The diagram above shows how the values are printed in the matrix.
Note that the remaining spaces in the matrix are filled with -1.

**Example 2:**

![|350](https://assets.leetcode.com/uploads/2022/05/11/ex2.jpg)

**Input:** m = 1, n = 4, head = `[0,1,2]`
**Output:** `[[0,1,2,-1]]`
**Explanation:** The diagram above shows how the values are printed from left to right in the matrix.
The last space in the matrix is set to -1.

**Constraints:**

- `1 <= m, n <= 105`
- `1 <= m * n <= 105`
- The number of nodes in the list is in the range `[1, m * n]`.
- `0 <= Node.val <= 1000`

#### Solution
##### Optimal Approach

This solution fills a 2D matrix in a spiral pattern using the values from a singly linked list. The matrix has dimensions `n x m`, and the linked list's values are inserted in a clockwise spiral fashion starting from the top-left corner.

1. **Define Directional Movement:**
    - The `dir` array stores four directions for movement: right, down, left, and up, which are essential to traverse the matrix in a spiral manner.
2. **Initialize Matrix:**
    - The matrix is initialized with `-1` to mark unfilled spots. This helps in checking if a cell has already been visited.
3. **Movement Logic:**    
    - Start at a position just outside the first cell (at `(0, -1)`).
    - In each iteration, calculate the next cell based on the current direction using the `dir` array.
    - If the next cell is within bounds and hasn't been visited (i.e., it's still `-1`), move there and insert the linked list value. If not, change direction clockwise and recalculate the next position.
4. **Direction Change:**    
    - If the next calculated position is out of bounds or already visited, rotate to the next direction in the spiral order by incrementing the direction index. The direction alternates between right, down, left, and up.
5. **Fill Until List Ends:**    
    - Continue this process until all nodes in the linked list have been placed into the matrix. The function then returns the spiral-filled matrix.

*TC ->* O( `m * n` )
*SC ->* O( 1 ), excluding space for result

```cpp title=Code
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
    int dir[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}}; // Right Down Left Up

    bool isSafe(vector<vector<int>>& res, int n, int m, int nx, int ny) {
        if (nx < 0 || nx >= n || ny < 0 || ny >= m || res[nx][ny] != -1)
            return false;

        return true;
    }

public:
    vector<vector<int>> spiralMatrix(int n, int m, ListNode* head) {
        vector<vector<int>> res(n, vector<int>(m, -1));

        int x = 0, y = -1, dirIndex = 0;

        while (head != nullptr) {
            int nx = x + dir[dirIndex][0];
            int ny = y + dir[dirIndex][1];

            if (isSafe(res, n, m, nx, ny)) {
                x = nx;
                y = ny;
                res[x][y] = head->val;
                head = head->next;
            } else {
                dirIndex = (dirIndex + 1) % 4;
            }
        }
        return res;
    }
};
```