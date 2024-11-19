---
difficulty: medium
leetcode-num: 199
topics:
  - Binary Tree
  - Depth-First Search
  - Breadth-First Search
---
[Problem Link](https://leetcode.com/problems/binary-tree-right-side-view/)

#### Problem
Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return _the values of the nodes you can see ordered from top to bottom_.

**Example 1:**

![|200](https://assets.leetcode.com/uploads/2021/02/14/tree.jpg)

**Input:** root = `[1,2,3,null,5,null,4]`
**Output:** `[1,3,4]`

**Example 2:**

**Input:** root = `[1,null,3]`
**Output:** `[1,3]`

**Example 3:**

**Input:** root = `[]`
**Output:**` []`

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

#### Solution


##### Recursive - DFS - Optimal Approach
[Video Explanation](https://youtu.be/KV4mRzTjlAk?list=PLgUwDviBIf0q8Hkd7bK2Bpryj2xVJk8Vk)

The idea is that we will traverse the Tree in such a way that we visit the right node first recursively. 
We will also have a parameter `level` in our recursive function. This will keep track of the current depth.

We will use the size of the `rightView` array to check if the current node is the rightmost node of a given depth as the rightmost node will appear first in the recursion. If the size of `rightView` is equal to `level`.. it means it's the rightmost node. All the other nodes in that level will be left of this node.

*TC ->* O( n )
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
    vector<int> rightView;

    void rightLeftTraversal(TreeNode* root, int level = 0) {
        if (root == nullptr)
            return;
        // push to result if this is the first discovered node of the current
        // level
        if (level == rightView.size())
            rightView.push_back(root->val);

        // traverse right first and then left recursively
        rightLeftTraversal(root->right, level + 1);
        rightLeftTraversal(root->left, level + 1);
    }

public:
    vector<int> rightSideView(TreeNode* root) {
        rightLeftTraversal(root);
        return rightView;
    }
};
```

##### Iterative - BFS Approach

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
vector<int> rightSideView(TreeNode* root) {
    vector<int> rightView;
    if (!root) return rightView;  // Handle null root

    queue<TreeNode*> q;
    q.push(root);

    while (!q.empty()) {
        int levelSize = q.size();
        for (int i = 0; i < levelSize; i++) {
            TreeNode* curr = q.front();
            q.pop();

            // If it's the last node of the level, add to rightView
            if (i == levelSize - 1) {
                rightView.push_back(curr->val);
            }

            if (curr->left) q.push(curr->left);
            if (curr->right) q.push(curr->right);
        }
    }

    return rightView;
}
```