---
difficulty: medium
leetcode-num: 2641
topics:
  - Binary Tree
  - Breadth-First Search
---
[Problem Link](https://leetcode.com/problems/cousins-in-binary-tree-ii/)

#### Problem

Given the `root` of a binary tree, replace the value of each node in the tree with the **sum of all its cousins' values**.

Two nodes of a binary tree are **cousins** if they have the same depth with different parents.

Return _the_ `root` _of the modified tree_.

**Note** that the depth of a node is the number of edges in the path from the root node to it.

**Example 1:**

![|600](https://assets.leetcode.com/uploads/2023/01/11/example11.png)

**Input:** root = [5,4,9,1,10,null,7]
**Output:** [0,0,0,7,7,null,11]
**Explanation:** The diagram above shows the initial binary tree and the binary tree after changing the value of each node.
- Node with value 5 does not have any cousins so its sum is 0.
- Node with value 4 does not have any cousins so its sum is 0.
- Node with value 9 does not have any cousins so its sum is 0.
- Node with value 1 has a cousin with value 7 so its sum is 7.
- Node with value 10 has a cousin with value 7 so its sum is 7.
- Node with value 7 has cousins with values 1 and 10 so its sum is 11.

**Example 2:**

![|500](https://assets.leetcode.com/uploads/2023/01/11/diagram33.png)

**Input:** root = [3,1,2]
**Output:** [0,0,0]
**Explanation:** The diagram above shows the initial binary tree and the binary tree after changing the value of each node.
- Node with value 3 does not have any cousins so its sum is 0.
- Node with value 1 does not have any cousins so its sum is 0.
- Node with value 2 does not have any cousins so its sum is 0.

**Constraints:**

- The number of nodes in the tree is in the range `[1, 105]`.
- `1 <= Node.val <= 104`
#### Solution
[Video Explanation](https://youtu.be/UIrargaZ61M)

##### Single Pass Approach
- **Initial Setup and Base Case:**
    
    - If the `root` is `null`, return `root` directly. Otherwise, initialize a queue `q` and start by pushing the `root` node into the queue.
    - Initialize `levelSum` to the value of the `root`. This variable will hold the sum of node values at the current level, which is used to calculate the new values for the nodes.
- **Breadth-First Search (BFS) Traversal:**
    
    - Perform a level-order traversal using a queue. This allows us to process nodes level by level.
    - For each level, keep track of the sum of values in the next level (`nextLevelSum`) to use for the upcoming level's nodes.
- **Processing Each Node at the Current Level:**
    
    - For each node at the current level, subtract the node's value from the `levelSum` to replace it with the sum of its cousins. The updated value becomes the difference between the total sum of the current level (`levelSum`) and the node's value.
    - Then, calculate the sum of the node's children (`childrenSum`). This is necessary because we will replace the children's values with this sum (since siblings are not cousins).
- **Update Children:**
    
    - For each child (left and right), set its value to `childrenSum`. This ensures that the child's value is set to the sum of its sibling(s), as siblings are not considered cousins.
    - As we process the children, accumulate their original values into `nextLevelSum` to track the sum for the next level.
- **Prepare for the Next Level:**
    
    - After processing all nodes at the current level, update `levelSum` to `nextLevelSum` and move on to the next level by continuing the loop.
- **Return the Modified Tree:**
    
    - After all levels have been processed and the tree is modified in place, return the `root`.

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {

public:
    TreeNode* replaceValueInTree(TreeNode* root) {
        queue<TreeNode*> q;

        if (!root)
            return root;

        q.push(root);
        int levelSum = root->val;

        while (!q.empty()) {
            int levelSize = q.size();
            int nextLevelSum = 0;

            for (int i = 0; i < levelSize; i++) {
                TreeNode* curr = q.front();
                q.pop();

                curr->val = levelSum - curr->val;
                int childrenSum = 0;

                if (curr->left) {
                    childrenSum += curr->left->val;
                    q.push(curr->left);
                }

                if (curr->right) {
                    childrenSum += curr->right->val;
                    q.push(curr->right);
                }

                if (curr->left) {
                    nextLevelSum += curr->left->val;
                    curr->left->val = childrenSum;
                }
                if (curr->right) {
                    nextLevelSum += curr->right->val;
                    curr->right->val = childrenSum;
                }
            }
            levelSum = nextLevelSum;
        }

        return root;
    }
};
```

##### Two Pass Approach
- **Precompute Level Sums:**
    
    - **Purpose:** The main idea is to replace each node's value with the sum of all the other nodes at the same level (excluding its own children). To achieve this, we first compute the sum of values at each level of the tree.
    - **How:** We use a `queue` to perform a **Breadth-First Search (BFS)** traversal, and for each level, we calculate the total sum of node values (`levelSum`). We store the sum of each level in the `levelSums` vector.
- **Initial Setup for BFS Traversal:**
    
    - Start by pushing the `root` node into the queue and initialize the `levelSums` vector, which will store the sum of node values for each level.
    - As we traverse the tree, we sum the node values level by level and store these sums in `levelSums`.
- **Modify the Node Values:**
    
    - After precomputing the level sums, we again use BFS to modify the node values.
    - **Root Special Case:** The root's value is set to `0` because the root doesn't have any cousins.
    - **Replace the Value of Each Node:**
        - For each node, we calculate the sum of its children (if any).
        - The node's value is then replaced by subtracting the sum of its children from the precomputed level sum of the next level. This effectively gives the sum of all nodes at the next level except for the current node's children.
- **Iterate Over Levels:**
    
    - For each node at a given level, calculate the new values for its children based on the difference between the level sum and its own children's values. This ensures that each node's value is replaced by the sum of its cousins.
- **Return the Modified Tree:**
    
    - After modifying the tree in place, the function returns the modified root.

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

public:
    TreeNode* replaceValueInTree(TreeNode* root) {
        vector<int> levelSums;
        queue<TreeNode*> q;

        if (!root)
            return root;

        q.push(root);

        while (!q.empty()) {
            int levelSize = q.size();
            int levelSum = 0;
            for (int i = 0; i < levelSize; i++) {
                TreeNode* curr = q.front();
                q.pop();

                levelSum += curr->val;

                if (curr->left)
                    q.push(curr->left);

                if (curr->right)
                    q.push(curr->right);
            }
            levelSums.push_back(levelSum);
        }

        q.push(root);
        root->val = 0; // root will never have cousins

        int level = 0;
        while (!q.empty()) {
            int levelSize = q.size();
            for (int i = 0; i < levelSize; i++) {
                TreeNode* curr = q.front();
                q.pop();
                int childrenSum = 0;

                if (curr->left) {
                    childrenSum += curr->left->val;
                    q.push(curr->left);
                }

                if (curr->right) {
                    childrenSum += curr->right->val;
                    q.push(curr->right);
                }

                if (curr->left) {
                    curr->left->val = levelSums[level + 1] - childrenSum;
                }
                if (curr->right) {
                    curr->right->val = levelSums[level + 1] - childrenSum;
                }
            }
            level++;
        }

        return root;
    }
};
```