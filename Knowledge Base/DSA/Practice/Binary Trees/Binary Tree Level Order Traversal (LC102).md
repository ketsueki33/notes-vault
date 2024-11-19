---
difficulty: medium
leetcode-num: 102
topics:
  - Binary Tree
  - Breadth-First Search
---
[Problem Link](https://leetcode.com/problems/binary-tree-level-order-traversal/)

#### Problem
Given the `root` of a binary tree, return _the level order traversal of its nodes' values_. (i.e., from left to right, level by level).

**Example 1:**

![|200](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

**Input:** root = `[3,9,20,null,null,15,7]`
**Output:** `[[3],[9,20],[15,7]]`

**Example 2:**

**Input:** root = `[1]`
**Output:** `[[1]]`

**Example 3:**

**Input:** root = `[]`
**Output:** `[]`

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-1000 <= Node.val <= 1000`

#### Solution
##### Optimal Approach

We will maintain a queue and push the root first.

Inside the while loop:
- We will first count the nodes in the queue. These nodes belong to the current level.
- Initialize a temporary vector.
- The for loop makes sure we are processing the nodes in the same level. Inside the for loop:
	- push the values into the temporary vector
	- push left and right nodes into the queue if they exist.
- Push the temporary vector into the result.

Return the result.

*TC ->* O( n )
*SC ->* O( w ), where w is max width of tree

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
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> traversal;

        queue<TreeNode*> q;
        
        if (root != nullptr)
            q.push(root);

        while (!q.empty()) {
            int levelSize = q.size();

            vector<int> v;

            for (int i = 0; i < levelSize; i++) {
                TreeNode* curr = q.front();
                q.pop();

                v.push_back(curr->val);

                if (curr->left)
                    q.push(curr->left);

                if (curr->right)
                    q.push(curr->right);
            }
            traversal.push_back(v);
        }
        return traversal;
    }
};
```