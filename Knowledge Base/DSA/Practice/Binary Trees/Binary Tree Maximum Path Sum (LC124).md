---
difficulty: hard
leetcode-num: 124
topics:
  - Binary Tree
  - Depth-First Search
---
[Problem Link](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

#### Problem
A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return _the maximum **path sum** of any **non-empty** path_.

**Example 1:**

![|250](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)

**Input:** root = [1,2,3]
**Output:** 6
**Explanation:** The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.

**Example 2:**

![|300](https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg)

**Input:** root = [-10,9,20,null,null,15,7]
**Output:** 42
**Explanation:** The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.

**Constraints:**

- The number of nodes in the tree is in the range `[1, 3 * 104]`.
- `-1000 <= Node.val <= 1000`

#### Solution
[Video Explanation](https://youtu.be/WszrfSwMz58)

##### Optimal Approach
###### 1. **Global Variable: `res`**

- This variable stores the maximum path sum found so far. It is initialized to the smallest possible integer (`INT_MIN`) to ensure that any valid path sum will be larger than this initial value.

###### 2. **Recursive Function: `solve(TreeNode* root)`**

This function calculates the maximum path sum **that can end at the current node** and also updates the global `res` for the overall maximum path sum.

- **Base Case:**
    
    - If `root` is `nullptr`, the function returns 0, meaning there's no contribution to the path sum from a non-existent node.
- **Recursive Case:**
    
    - The function recursively calculates the maximum path sums from the left and right subtrees:
        
        - `left = solve(root->left)` calculates the maximum path sum from the left subtree.
        - `right = solve(root->right)` calculates the maximum path sum from the right subtree.
    - **At each node, the function updates the value at that node to account for its potential contribution to a path:**
        
        - `value = root->val`: Start with the value of the current node.
        - If the left subtree contributes positively (`left > 0`), add it to the current node’s value (`value += left`).
        - If the right subtree contributes positively (`right > 0`), add it to the current node’s value (`value += right`).
    - **Update the Global Result (`res`):**
        
        - `res = max(res, value)` updates the global result to hold the maximum path sum found so far, including the current node.
    - **Return the Maximum Contribution from the Current Node:**
        
        - The recursive call returns the maximum sum that **can be extended upwards** from the current node. It can either:
            1. Include just the node itself (`root->val`).
            2. Include the node and the left subtree (`root->val + left`).
            3. Include the node and the right subtree (`root->val + right`).
    - The return value helps build paths that can include the parent node in the recursion.
        

###### 3. **Main Function: `maxPathSum(TreeNode* root)`**

- This function initiates the recursion by calling `solve(root)` to process the entire tree.
- After the recursion completes, it returns the maximum path sum stored in `res`.


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
    int res = INT_MIN;

    int solve(TreeNode* root) {
        if (!root)
            return 0;

        int left = solve(root->left);
        int right = solve(root->right);

        int value = root->val;
        if (left > 0)
            value += left;

        if (right > 0)
            value += right;

        res = max(res, value);

        return max(root->val, max(root->val + left, root->val + right));
    }

public:
    int maxPathSum(TreeNode* root) {
        solve(root);

        return res;
    }
};
```