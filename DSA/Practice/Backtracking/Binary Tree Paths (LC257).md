---
difficulty: easy
leetcode-num: 257
topics:
  - Backtracking
  - Binary Tree
  - Depth-First Search
---
[Problem Link](https://leetcode.com/problems/binary-tree-paths/)

#### Problem
Given the `root` of a binary tree, return _all root-to-leaf paths in **any order**_.

A **leaf** is a node with no children.

**Example 1:**

![|200](https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg)

**Input:** root = `[1,2,3,null,5]`
**Output:** `["1->2->5","1->3"]`

**Example 2:**

**Input:** root =` [1]`
**Output:** `["1"]`

**Constraints:**

- The number of nodes in the tree is in the range `[1, 100]`.
- `-100 <= Node.val <= 100`

#### Solution

We maintain a `vector<vector<int>> paths` to keep track of valid paths. 

`currPath`  keeps track of current path.

**findPaths Function:**
- This function performs a depth-first search (DFS) on the tree to collect all the paths from the root to the leaf nodes.
- It takes two arguments:
    - `root`: The current node being processed.
    - `currPath`: A vector that stores the current path from the root to the current node.


- If the current node (`root`) is `nullptr`, the function returns.
- It adds the current node's value to `currPath`.
- If the current node is a **leaf node** (i.e., it has no left or right child), the current path is copied to the `paths` vector.
- Then, the function recursively explores both the left and right children.
- After visiting a node and its children, the function removes the current node's value from `currPath` to backtrack.

**binaryTreePaths Function:**

- This function is responsible for converting the collected paths (stored as integer vectors) into string representations.
- It calls `findPaths` to gather all paths.
- After paths are collected, it loops through each path in `paths`:
    - It creates a string by concatenating node values with "->" between them.
    - It removes the last two characters (the trailing "->").
    - The final string is pushed into the result vector `res`.
- Finally, it returns the result `res`, which contains all the root-to-leaf paths as strings.


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
    vector<vector<int>> paths;

    void findPaths(TreeNode* root, vector<int>& currPath) {
        if (!root)
            return;

        currPath.push_back(root->val);

        if (!root->left && !root->right)
            paths.push_back(currPath);

        findPaths(root->left, currPath);
        findPaths(root->right, currPath);

        currPath.pop_back();
    }

public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<int> currPath;
        vector<string> res;
        findPaths(root, currPath);

        for (vector<int>& v : paths) {
            string s = "";
            for (int x : v)
                s += to_string(x) + "->";
            s.pop_back();
            s.pop_back();
            res.push_back(s);
        }
        return res;
    }
};
```