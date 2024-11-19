---
difficulty: medium
leetcode-num: 105
topics:
  - Array
  - Hash Table
  - Binary Tree
  - Divide and Conquer
---
[Problem Link](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

#### Problem
Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return _the binary tree_.

**Example 1:**

![|200](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

**Input:** preorder = `[3,9,20,15,7]`, inorder = `[9,3,15,20,7]`
**Output:** `[3,9,20,null,null,15,7]
`
**Example 2:**

**Input:** preorder = `[-1]`, inorder = `[-1]`
**Output:** `[-1]`

**Constraints:**

- `1 <= preorder.length <= 3000`
- `inorder.length == preorder.length`
- `-3000 <= preorder[i], inorder[i] <= 3000`
- `preorder` and `inorder` consist of **unique** values.
- Each value of `inorder` also appears in `preorder`.
- `preorder` is **guaranteed** to be the preorder traversal of the tree.
- `inorder` is **guaranteed** to be the inorder traversal of the tree.

#### Solution

##### Optimal Approach
We will use the Preorder traversal to determine the root of the tree.

After we find the root, we will use the Inorder traversal to determine which nodes lie on the left and right subtrees of the node.

We will use a hash map to find the `rootIndex` in O( 1 ) time.

We can then break the problem into a sub-problem and construct the tree recursively.

**Important thing** to note how `preIndex` is used to get the root of the current subtree every time.
Since we will recursively call `createTree` for left subtree first, `preorder[preIndex]` will give the root of the current subtree if we increment `preIndex` by 1 each time.

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {
    int preIndex = 0;
    unordered_map<int, int> inorderIndex;

    TreeNode* createTree(vector<int>& inorder, int in_start, int in_end,
                         vector<int>& preorder) {
        if (in_end < in_start)
            return nullptr;

        int rootVal = preorder[preIndex++];
        int rootIndex = inorderIndex[rootVal];

        TreeNode* root = new TreeNode(rootVal);

        root->left = createTree(inorder, in_start, rootIndex - 1, preorder);
        root->right = createTree(inorder, rootIndex + 1, in_end, preorder);

        return root;
    }

public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        for (int i = 0; i < preorder.size(); i++) {
            inorderIndex[inorder[i]] = i;
        }

        return createTree(inorder, 0, preorder.size() - 1, preorder);
    }
};
```

##### Alright Approach
We will use the Preorder traversal to determine the root of the tree.

After we find the root, we will use the Inorder traversal to determine which nodes lie on the left and right subtrees of the node.

We can then break the problem into a sub-problem and construct the tree recursively.

**Important thing** to note how `preIndex` is used to get the root of the current subtree every time.
Since we will recursively call `createTree` for left subtree first, `preorder[preIndex]` will give the root of the current subtree if we increment `preIndex` by 1 each time.

*TC ->* O( n$^{2}$ )
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
    int preIndex = 0;

    TreeNode* createTree(vector<int>& inorder, int in_start, int in_end,
                         vector<int>& preorder) {
        if (in_end < in_start)
            return nullptr;

        int rootVal = preorder[preIndex++];
        int rootIndex;

        for (int i = in_start; i <= in_end; i++) {
            if (rootVal == inorder[i]) {
                rootIndex = i;
                break;
            }
        }

        TreeNode* root = new TreeNode(rootVal);

        root->left = createTree(inorder, in_start, rootIndex - 1, preorder);
        root->right = createTree(inorder, rootIndex + 1, in_end, preorder);

        return root;
    }

public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return createTree(inorder, 0, preorder.size() - 1, preorder);
    }
};
```