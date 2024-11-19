---
difficulty: medium
leetcode-num: 106
topics:
  - Array
  - Hash Table
  - Divide and Conquer
  - Binary Tree
---
[Problem Link](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

#### Problem
Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return _the binary tree_.

**Example 1:**

![|200](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

**Input:** inorder = `[9,3,15,20,7],` postorder = `[9,15,7,20,3]`
**Output:** `[3,9,20,null,null,15,7]`

**Example 2:**

**Input:** inorder = `[-1]`, postorder = `[-1]`
**Output:** `[-1]`

**Constraints:**

- `1 <= inorder.length <= 3000`
- `postorder.length == inorder.length`
- `-3000 <= inorder[i], postorder[i] <= 3000`
- `inorder` and `postorder` consist of **unique** values.
- Each value of `postorder` also appears in `inorder`.
- `inorder` is **guaranteed** to be the inorder traversal of the tree.
- `postorder` is **guaranteed** to be the postorder traversal of the tree.

#### Solution
The solution is similar to [[Construct Binary Tree from Preorder and Inorder Traversal (LC105)#Solution|creating a tree from preorder and inorder traversal]]  with some minor differences.

We use **`postIndex`** which is initialized to `postorder.size()-1` instead of `0`.  It is because the root will always be the rightmost element.

We will call `createTree` for right subtree before the left subtree so that the root of subtrees coincide with the current `postIndex`.
##### Optimal Approach
We will use the Postorder traversal to determine the root of the tree.

After we find the root, we will use the Inorder traversal to determine which nodes lie on the left and right subtrees of the node.

We will use a hash map to find the `rootIndex` in O( 1 ) time.

We can then break the problem into a sub-problem and construct the tree recursively.

**Important thing** to note how `postIndex` is used to get the root of the current subtree every time.
Since we will recursively call `createTree` for right subtree first, `postorder[postIndex]` will give the root of the current subtree if we decrement `postIndex` by 1 each time.

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {
    int postIndex;
    unordered_map<int, int> inorderIndex;

    TreeNode* createTree(vector<int>& inorder, int in_start, int in_end,
                         vector<int>& postorder) {
        if (in_end < in_start)
            return nullptr;

        int rootVal = postorder[postIndex--];
        int rootIndex = inorderIndex[rootVal];

        TreeNode* root = new TreeNode(rootVal);

        root->right = createTree(inorder, rootIndex + 1, in_end, postorder);
        root->left = createTree(inorder, in_start, rootIndex - 1, postorder);

        return root;
    }

public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        postIndex = postorder.size() - 1;
        for (int i = 0; i < inorder.size(); i++) {
            inorderIndex[inorder[i]] = i;
        }
        return createTree(inorder, 0, postorder.size() - 1, postorder);
    }
};
```

##### Basic Approach
We will use the Postorder traversal to determine the root of the tree.

After we find the root, we will use the Inorder traversal to determine which nodes lie on the left and right subtrees of the node.

We can then break the problem into a sub-problem and construct the tree recursively.

**Important thing** to note how `postIndex` is used to get the root of the current subtree every time.
Since we will recursively call `createTree` for right subtree first, `postorder[postIndex]` will give the root of the current subtree if we decrement `postIndex` by 1 each time.

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
    int postIndex;

    TreeNode* createTree(vector<int>& inorder, int in_start, int in_end,
                         vector<int>& postorder) {
        if (in_end < in_start)
            return nullptr;

        int rootVal = postorder[postIndex--];
        int rootIndex;

        for (int i = in_start; i <= in_end; i++) {
            if (rootVal == inorder[i]) {
                rootIndex = i;
                break;
            }
        }

        TreeNode* root = new TreeNode(rootVal);

        root->right = createTree(inorder, rootIndex + 1, in_end, postorder);
        root->left = createTree(inorder, in_start, rootIndex - 1, postorder);

        return root;
    }

public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        postIndex = postorder.size() - 1;

        return createTree(inorder, 0, postorder.size() - 1, postorder);
    }
};
```