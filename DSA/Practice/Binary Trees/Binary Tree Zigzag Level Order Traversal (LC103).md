---
difficulty: medium
leetcode-num: 103
topics:
  - Binary Tree
  - Breadth-First Search
---
[Problem Link](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

#### Problem
Given the `root` of a binary tree, return _the zigzag level order traversal of its nodes' values_. (i.e., from left to right, then right to left for the next level and alternate between).

**Example 1:**

![|150](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

**Input:** root = `[3,9,20,null,null,15,7]`
**Output:** `[[3],[20,9],[15,7]]`

**Example 2:**

**Input:** root = `[1]`
**Output:** `[[1]]`

**Example 3:**

**Input:** root = `[]`
**Output:** `[]`

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-100 <= Node.val <= 100`

#### Solution

##### Actual ZigZag Traversal Approach
- We use two stacks: `s1` and `s2`.
- We start by pushing the root into `s1`.
- We use a boolean `leftToRight` to keep track of the current traversal direction.
- In each iteration of the outer while loop:
    - If going left to right:
        - Pop nodes from `s1`
        - Add their values to the current level
        - Push their children to `s2` (left first, then right)
    - If going right to left:
        - Pop nodes from `s2`
        - Add their values to the current level
        - Push their children to `s1` (right first, then left)
- After processing each level, we flip the `leftToRight` boolean.
- We continue this process until both stacks are empty.

*TC ->* O( n )
*SC ->* O( w ), where w is max width of binary tree

```cpp title=Code
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> result;
        if (!root)
            return result;

        stack<TreeNode*> s1, s2;
        s1.push(root);
        bool leftToRight = true;

        while (!s1.empty() || !s2.empty()) {
            vector<int> currentLevel;

            if (leftToRight) {
                while (!s1.empty()) {
                    TreeNode* node = s1.top();
                    s1.pop();
                    currentLevel.push_back(node->val);

                    if (node->left)
                        s2.push(node->left);
                    if (node->right)
                        s2.push(node->right);
                }
            } else {
                while (!s2.empty()) {
                    TreeNode* node = s2.top();
                    s2.pop();
                    currentLevel.push_back(node->val);

                    if (node->right)
                        s1.push(node->right);
                    if (node->left)
                        s1.push(node->left);
                }
            }

            result.push_back(currentLevel);
            leftToRight = !leftToRight;
        }

        return result;
    }
};
```

##### Normal Level Order Traversal Approach
Traverse the tree like in a normal [[Binary Tree Level Order Traversal (LC102)|level order traversal]] but keep track of the current level. If the level is odd, reverse the temporary vector before pushing it into the result.

Alternatively you could declare the size of the temporary vector as  `levelSize` during initialization. Then inside the for loop depending on whether the current level is odd or even, place elements at either `i` or `levelSize - i - 1`.

*TC ->* O( n )
*SC ->* O( w ), where w is max width of binary tree

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
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> traversal;

        queue<TreeNode*> q;

        if (root != nullptr)
            q.push(root);

        int level = 0;

        while (!q.empty()) {
            int levelSize = q.size();

            vector<int> v;

            for (int i = 0; i < levelSize; i++) {
                TreeNode* curr = q.front();
                q.pop();

                v.push_back(curr->val);

                if (curr->left)
                    q.push(curr->left);

                if (curr->right)
                    q.push(curr->right);
            }
            if (level % 2 != 0)
                reverse(v.begin(), v.end());

            traversal.push_back(v);
            level++;
        }
        return traversal;
    }
};
```
