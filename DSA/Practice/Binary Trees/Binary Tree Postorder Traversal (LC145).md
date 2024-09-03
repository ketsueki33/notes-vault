---
difficulty: easy
leetcode-num: 145
topics:
  - Stack
  - Depth-First Search
  - Binary Tree
---
[Problem Link](https://leetcode.com/problems/binary-tree-postorder-traversal/)

#### Problem
Given the `root` of a binary tree, return _the postorder traversal of its nodes' values_.

**Example 1:**

![|100](https://assets.leetcode.com/uploads/2020/08/28/pre1.jpg)

**Input:** root =` [1,null,2,3]`
**Output:** `[3,2,1]`

**Example 2:**

**Input:** root = []
**Output:** []

**Example 3:**

**Input:** root = `[1]`
**Output:** `[1]`
**Constraints:**

- The number of the nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

#### Solution
Postorder traversal follows the following order :

1. Left Node
2. Right Node
3. Root Node

##### Recursive Approach

We create a recursive function which first calls itself for left node, and then calls itself for right node and at last pushes the value of root into the result vector first.

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

    void postorder(TreeNode* root) {
        if (!root)
            return;

        postorder(root->left);
        postorder(root->right);
        traversal.push_back(root->val);
    }

public:
    vector<int> postorderTraversal(TreeNode* root) {
        postorder(root);
        return traversal;
    }
};

```

##### Iterative Approach
This iterative approach mimics the behavior of the recursive pre-order traversal. 

###### Using 1 Stack
[Video Explanation](https://youtu.be/NzIGLLwZBS8)

*TC ->* O( n )
*SC ->* O( h )  (where h is height of the tree)

```cpp title=Code
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> traversal;
        TreeNode* curr = root;

        while (curr != nullptr || !st.empty()) {
            while (curr != nullptr) {
                st.push(curr);
                curr = curr->left;
            }

            TreeNode* temp = st.top()->right;
            if (temp == nullptr) {
                temp = st.top();
                st.pop();
                traversal.push_back(temp->val);
                while (!st.empty() && temp == st.top()->right) {
                    temp = st.top();
                    st.pop();
                    traversal.push_back(temp->val);
                }
            } else
                curr = temp;
        }
        return traversal;
    }
};
```

###### Using 2 Stacks
[Video Explanation](https://youtu.be/2YBhNLodD8Q)
*TC ->* O( n )
*SC ->* O( n ) 

```cpp title=Code
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> st1, st2;
        vector<int> traversal;

        if( root == nullptr )
            return traversal;

        TreeNode* curr;
        st1.push(root);
        
        while( !st1.empty() ){
            curr = st1.top();
            st1.pop();
            st2.push(curr);
            if(curr-> left)
                st1.push(curr->left);
            if(curr->right)
                st1.push(curr->right);
        }

        while(!st2.empty()){
            traversal.push_back(st2.top()->val);
            st2.pop();
        }
        return traversal;

    }
};
```