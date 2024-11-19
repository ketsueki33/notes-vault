---
difficulty: easy
leetcode-num: 144
topics:
  - Stack
  - Binary Tree
  - Depth-First Search
---
[Problem Link](https://leetcode.com/problems/binary-tree-preorder-traversal/)

#### Problem
Given the `root` of a binary tree, return _the preorder traversal of its nodes' values_.

**Example 1:**

![|100](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)

**Input:** root = `[1,null,2,3]`
**Output:** `[1,2,3]`

**Example 2:**

**Input:** root = `[]`
**Output:** `[]`

**Example 3:**

**Input:** root = `[1]`
**Output:** `[1]`

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

#### Solution

Preorder traversal follows the following order :

1. Root Node
2. Left Node
3. Right Node

##### Recursive Approach

We create a recursive function which  pushes the value of root into the result vector first, then calls itself for left node, and then calls itself for right node.  

_TC ->_ O( n )  
_SC ->_ O( h ) (where h is height of the tree)


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
    vector<int> traversal;

    void preorder(TreeNode* root) {
        if (!root)
            return;

        traversal.push_back(root->val);
        preorder(root->left);
        preorder(root->right);
    }

public:
    vector<int> preorderTraversal(TreeNode* root) {
        preorder(root);
        return traversal;
    }
};
```

##### Iterative Approach
This iterative approach mimics the behavior of the recursive pre-order traversal. 

Inside the while loop, we push the value to the result first and then travel to the left node.

When `curr` becomes `NULL`.. then we move to the right node of the topmost node in the stack.

*TC ->* O( n )
*SC ->* O( h )  (where h is height of the tree)

```cpp title=Code
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> traversal;
        TreeNode* curr = root;

        while( curr != nullptr || !st.empty() ){
            while(curr!= nullptr){
                traversal.push_back(curr->val);
                st.push(curr);
                curr = curr->left;
            }

            curr = st.top();
            st.pop();
            curr = curr->right;
        }
        return traversal;
    }
};
```