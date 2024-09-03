---
difficulty: medium
leetcode-num: 236
topics:
  - Binary Tree
  - Depth-First Search
---
[Problem Link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

#### Problem
Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

**Example 1:**

![|200](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Input:** root = `[3,5,1,6,2,0,8,null,null,7,4]`, p = 5, q = 1
**Output:** 3
**Explanation:** The LCA of nodes 5 and 1 is 3.

**Example 2:**

![|200](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Input:** root = `[3,5,1,6,2,0,8,null,null,7,4]`, p = 5, q = 4
**Output:** 5
**Explanation:** The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.

**Example 3:**

**Input:** root = `[1,2]`, p = 1, q = 2
**Output:** 1

**Constraints:**

- The number of nodes in the tree is in the range `[2, 105]`.
- `-109 <= Node.val <= 109`
- All `Node.val` are **unique**.
- `p != q`
- `p` and `q` will exist in the tree.

#### Solution
We will use recursion while keeping the following things in mind:
- If `root` is same as `p` or `q` ... return `root`.
- Check for `p` and `q` in left and right subtrees.
	- If one subtree contains `p` and other contains `q` then return `root`.
	- If one subtree contains both `p` and `q`.. then return the result of that subtree.
	- If neither contain `p` nor `q`.. return `nullptr`

*TC ->* O( n )
*SC ->* O( h ) where h is the height of the tree.

```cpp title=Code
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == nullptr || root == p || root == q)
            return root;

        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);

        if (left && right)
            return root;

        if (left)
            return left;

        if (right)
            return right;

        return nullptr;
    }
};
```