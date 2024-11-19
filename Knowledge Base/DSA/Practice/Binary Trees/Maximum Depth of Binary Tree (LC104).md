---
difficulty: easy
leetcode-num: 104
topics:
  - Binary Tree
  - Depth-First Search
---
[Problem Link](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

#### Problem
Given the `root` of a binary tree, return _its maximum depth_.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**

![|250](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

**Input:** root = `[3,9,20,null,null,15,7]`
**Output:** 3

**Example 2:**

**Input:** root = `[1,null,2]`
**Output:** 2

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `-100 <= Node.val <= 100`

#### Solution

##### Recursive Approach
In each recursive call, add 1 to height of left subtree if it's height is greater or the right  subtree otherwise.

*TC ->* O( n )
*SC ->* O( h ), where h is height of tree

```cpp title=Code
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if( !root ) return 0;

        return max(maxDepth(root->left), maxDepth(root->right))+1;
    }
};
```