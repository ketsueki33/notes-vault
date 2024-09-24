---
difficulty: easy
leetcode-num: 543
topics:
  - Binary Tree
  - Depth-First Search
---
[Problem Link](https://leetcode.com/problems/diameter-of-binary-tree/)

#### Problem
Given the `root` of a binary tree, return _the length of the **diameter** of the tree_.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

**Example 1:**

![|250](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

**Input:** root = `[1,2,3,4,5]`
**Output:** 3
**Explanation:** 3 is the length of the path `[4,2,1,3]` or `[5,2,1,3]`

**Example 2:**

**Input:** root = `[1,2]`
**Output:** 1

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-100 <= Node.val <= 100`
#### Solution
##### Optimal Approach
Solution is is similar to [[Maximum Depth of Binary Tree (LC104)| finding height of a Binary Tree]].

Only extra step is calculating diameter. Sum of height of left and right subtree is the diameter of that node.

*TC ->* O( n )
*SC ->* O( h ) , where h is height of tree

```cpp title=Code
class Solution {

    int recursiveHelper(TreeNode* root, int& diameter) {
        if (!root)
            return 0;

        int left = recursiveHelper(root->left, diameter);
        int right = recursiveHelper(root->right, diameter);

        diameter = max(diameter, left + right);

        return max(left, right) + 1;
    }

public:
    int diameterOfBinaryTree(TreeNode* root) {
        int diameter = 0;

        recursiveHelper(root, diameter);

        return diameter;
    }
};
```