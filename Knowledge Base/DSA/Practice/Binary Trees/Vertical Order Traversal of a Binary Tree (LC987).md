---
difficulty: hard
leetcode-num: 987
topics:
  - Hash Table
  - Breadth-First Search
  - Binary Tree
---
[Problem Link](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/)

#### Problem
Given the `root` of a binary tree, calculate the **vertical order traversal** of the binary tree.

For each node at position `(row, col)`, its left and right children will be at positions `(row + 1, col - 1)` and `(row + 1, col + 1)` respectively. The root of the tree is at `(0, 0)`.

The **vertical order traversal** of a binary tree is a list of top-to-bottom orderings for each column index starting from the leftmost column and ending on the rightmost column. There may be multiple nodes in the same row and same column. In such a case, sort these nodes by their values.

Return _the **vertical order traversal** of the binary tree_.

**Example 1:**

![|400](https://assets.leetcode.com/uploads/2021/01/29/vtree1.jpg)

**Input:** root = `[3,9,20,null,null,15,7]`
**Output:** `[[9],[3,15],[20],[7]]`
**Explanation:**
Column -1: Only node 9 is in this column.
Column 0: Nodes 3 and 15 are in this column in that order from top to bottom.
Column 1: Only node 20 is in this column.
Column 2: Only node 7 is in this column.

**Example 2:**

![|400](https://assets.leetcode.com/uploads/2021/01/29/vtree2.jpg)

**Input:** root = `[1,2,3,4,5,6,7]`
**Output:** `[[4],[2],[1,5,6],[3],[7]]`
**Explanation:**
Column -2: Only node 4 is in this column.
Column -1: Only node 2 is in this column.
Column 0: Nodes 1, 5, and 6 are in this column.
          1 is at the top, so it comes first.
          5 and 6 are at the same position (2, 0), so we order them by their value, 5 before 6.
Column 1: Only node 3 is in this column.
Column 2: Only node 7 is in this column.

**Example 3:**

![|400](https://assets.leetcode.com/uploads/2021/01/29/vtree3.jpg)

**Input:** root = `[1,2,3,4,6,5,7]`
**Output:** `[[4],[2],[1,5,6],[3],[7]]`
**Explanation:**
This case is the exact same as example 2, but with nodes 5 and 6 swapped.
Note that the solution remains the same since 5 and 6 are in the same location and should be ordered by their values.

**Constraints:**

- The number of nodes in the tree is in the range `[1, 1000]`.
- `0 <= Node.val <= 1000`

#### Solution

##### Optimal Approach
We perform a level-order traversal (BFS) while maintaining an `xIndex` (horizontal position) for each node. Along with the node, this `xIndex` is stored in the queue.

We will use a `map<int, vector<int>>`  (`mp`) to store the values for each `xIndex`. Map is idead here since it keeps the `xIndex` sorted automatically.

We could have pushed the values directly into `mp` but then nodes with same `xIndex` and `level` would cause problem. In this case , we have to make sure that the lower value appears first in the result. 

So to accommodate this condition, we will use an `unordered_map<int, multiset<int>>` (`lmp`) to temporarily hold the values of same `xIndex` together ( `level` will be same always in one iteration of while loop) inside the while loop. 

We have used `multiset` over `set` because node values can repeat and `set` doesn't hold duplicates. At the end of each while loop iteration, we will transfer the values from `lmp` to `mp` corresponding to each index. 

After the whole tree has been traversed.. we will construct the result. Each vertical column will appear from left to right as we have used a `map`. After pushing each `vector` in `mp` to `res`, we can return `res`.


*TC ->* O( n * log n )
*SC ->* O( n )

```cpp title=Code
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left),
 * right(right) {}
 * };
 */
 
class Solution {
    typedef pair<TreeNode*, int> P;

public:
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        queue<P> q;
        q.push({root, 0});

        map<int, vector<int>> mp;

        vector<vector<int>> res;

        while (!q.empty()) {
            int levelSize = q.size();
            unordered_map<int, multiset<int>> lmp;

            for (int i = 0; i < levelSize; i++) {
                TreeNode* curr = q.front().first;
                int xIndex = q.front().second;
                q.pop();
                lmp[xIndex].insert(curr->val);
                if (curr->left)
                    q.push({curr->left, xIndex - 1});

                if (curr->right)
                    q.push({curr->right, xIndex + 1});
            }
            
            for (auto x : lmp) {
                mp[x.first].insert(mp[x.first].end(), x.second.begin(),
                                   x.second.end());
            }
        }

        for (auto x : mp) {
            res.push_back(x.second);
        }

        return res;
    }
};
```