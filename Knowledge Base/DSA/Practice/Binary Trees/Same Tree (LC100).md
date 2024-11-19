---
difficulty: easy
leetcode-num: 100
topics:
  - Binary Tree
  - Depth-First Search
---
[Problem Link](https://leetcode.com/problems/same-tree/)

#### Problem
Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

**Example 1:**

![|300](https://assets.leetcode.com/uploads/2020/12/20/ex1.jpg)

**Input:**` p = [1,2,3], q = [1,2,3]`
**Output:** true

**Example 2:**

![|300](https://assets.leetcode.com/uploads/2020/12/20/ex2.jpg)

**Input:**` p = [1,2], q = [1,null,2]`
**Output:** false

**Example 3:**

![|300](https://assets.leetcode.com/uploads/2020/12/20/ex3.jpg)

**Input:** `p = [1,2,1], q = [1,1,2]`
**Output:** false

**Constraints:**

- The number of nodes in both trees is in the range `[0, 100]`.
- `-104 <= Node.val <= 104`

#### Solution
Define base cases and  return true if and only if :
- `p->val` is same as `q->val`
- p's left subtree is same as q's left subtree.
- p's right subtree is same as q's right subtree.

*TC ->* O( n )
*SC ->* O( h )

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
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (p == nullptr && q == nullptr)
            return true;

        if (p == nullptr || q == nullptr)
            return false;

        return (p->val == q->val && isSameTree(p->left, q->left) &&
                isSameTree(p->right, q->right));
    }
};
```