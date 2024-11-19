---
difficulty: easy
leetcode-num: 94
topics:
  - Binary Tree
  - Stack
  - Depth-First Search
---
[Problem Link](https://leetcode.com/problems/binary-tree-inorder-traversal/)

#### Problem
Given the `root` of a binary tree, return _the inorder traversal of its nodes' values_.

**Example 1:**

![|100](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)

**Input:** root = `[1,null,2,3]`
**Output:** `[1,3,2]`

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
Inorder traversal follows the following order :
1. Left Node
2. Root Node
3. Right Node

##### Recursive Approach
We create a recursive function which calls itself for left node first, then pushes the value of root into the result vector, and then calls itself for right node.
*TC ->* O( n )
*SC ->* O( h ) (where h is height of the tree)

```cpp title=Code
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
    vector<int> traversal;

    void inorder(TreeNode* root){
        if( !root ) return;

        inorder(root->left);
        traversal.push_back(root->val);
        inorder(root->right);
    }
public:
    vector<int> inorderTraversal(TreeNode* root) {
        inorder(root);
        return traversal;
    }
};
```

##### Iterative Approach
This iterative approach mimics the behavior of the recursive inorder traversal. Here's how it works:

1. We use a stack to keep track of the nodes we've visited but haven't processed yet.
2. We start with the root node as the current node.
3. We enter a loop that continues as long as we have a current node to process or there are nodes in the stack.
4. In each iteration:
    - We first traverse as far left as possible, pushing each node onto the stack.
    - When we can't go left anymore, we pop a node from the stack, process it (add its value to the traversal), and move to its right child.
5. This process continues until we've processed all nodes.
*TC ->* O( n )
*SC ->* O( h )  (where h is height of the tree)

```cpp title=Code
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> traversal;
        TreeNode* curr = root;

        while (curr != NULL || !st.empty()) {
	        // Traverse to the leftmost node
            while (curr != NULL) {
                st.push(curr);
                curr = curr->left;
            }
            
			// Process the current node
            curr = st.top();
            st.pop();
            traversal.push_back(curr->val);
            
			// Move to the right child
            curr = curr->right;
        }
        return traversal;
    }
};
```