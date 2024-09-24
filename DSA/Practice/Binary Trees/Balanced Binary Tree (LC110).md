---
difficulty: easy
leetcode-num: 110
topics:
  - Binary Tree
  - Depth-First Search
---
[Problem Link]()

#### Problem
Given a binary tree, determine if it is **height-balanced**.

**Example 1:**

![|250](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

**Input:** root = `[3,9,20,null,null,15,7]`
**Output:** true

**Example 2:**

![|300](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)

**Input:** root = `[1,2,2,3,3,null,null,4,4]`
**Output:** false

**Example 3:**

**Input:** root = `[]`
**Output:** true

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5000]`.
- `-104 <= Node.val <= 104`

#### Solution
- **Recursive `checkBalance` Function:**    
    - This function checks if a subtree is balanced and returns its height if it is.
    - It starts from the leaf nodes and calculates the height of each subtree recursively.
    - If a subtree is unbalanced, the function immediately returns `-1` to indicate imbalance.
- **Base Case:**    
    - If the current node (`root`) is `nullptr`, it means the subtree is empty, and an empty tree is balanced, so it returns a height of `0`.
- **Left and Right Subtree Heights:**    
    - For each node, it recursively calculates the height of the left and right subtrees by calling `checkBalance` on `root->left` and `root->right`.
- **Check for Imbalance:**    
    - If either the left or right subtree has returned `-1`, it means one of the subtrees is unbalanced. The function immediately propagates `-1` upwards, skipping further checks.
    - Otherwise, it checks if the absolute difference between the left and right subtree heights is greater than 1. If the difference is within the allowed range (<= 1), the tree is still balanced, and it returns the height of the current subtree (`max(left, right) + 1`). If not, it returns `-1` to indicate an imbalance.
- **Public `isBalanced` Function:**    
    - This function calls `checkBalance` on the root of the tree. If the result is less than 0 (meaning one or more subtrees are unbalanced), it returns `false`; otherwise, it returns `true`.
    
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
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
 
class Solution {
    int checkBalance(TreeNode* root){
        if(!root)
            return 0;

        int left = checkBalance(root->left);
        int right = checkBalance(root->right);

        if(left < 0 || right < 0 )
            return -1;

        if( abs(left-right) <= 1 )
            return max(left,right) + 1;
        else return -1;
    }
public:
    bool isBalanced(TreeNode* root) {
        if( checkBalance(root)< 0 )
            return false;
        else return true;
    }
};
```