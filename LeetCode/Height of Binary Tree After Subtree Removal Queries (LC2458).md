---
difficulty: hard
leetcode-num: 2458
topics:
  - Binary Tree
  - Depth-First Search
  - Array
---
[Problem Link](https://leetcode.com/problems/height-of-binary-tree-after-subtree-removal-queries/)

#### Problem
You are given the `root` of a **binary tree** with `n` nodes. Each node is assigned a unique value from `1` to `n`. You are also given an array `queries` of size `m`.

You have to perform `m` **independent** queries on the tree where in the `ith` query you do the following:

- **Remove** the subtree rooted at the node with the value `queries[i]` from the tree. It is **guaranteed** that `queries[i]` will **not** be equal to the value of the root.

Return _an array_ `answer` _of size_ `m` _where_ `answer[i]` _is the height of the tree after performing the_ `ith` _query_.

**Note**:

- The queries are independent, so the tree returns to its **initial** state after each query.
- The height of a tree is the **number of edges in the longest simple path** from the root to some node in the tree.

**Example 1:**

![|400](https://assets.leetcode.com/uploads/2022/09/07/binaryytreeedrawio-1.png)

**Input:** root = [1,3,4,2,null,6,5,null,null,null,null,null,7], queries = [4]
**Output:** [2]
**Explanation:** The diagram above shows the tree after removing the subtree rooted at node with value 4.
The height of the tree is 2 (The path 1 -> 3 -> 2).

**Example 2:**

![|300](https://assets.leetcode.com/uploads/2022/09/07/binaryytreeedrawio-2.png)

**Input:** root = [5,8,9,2,1,3,7,4,6], queries = [3,2,4,8]
**Output:** [3,2,3,2]
**Explanation:** We have the following queries:
- Removing the subtree rooted at node with value 3. The height of the tree becomes 3 (The path 5 -> 8 -> 2 -> 4).
- Removing the subtree rooted at node with value 2. The height of the tree becomes 2 (The path 5 -> 8 -> 1).
- Removing the subtree rooted at node with value 4. The height of the tree becomes 3 (The path 5 -> 8 -> 2 -> 6).
- Removing the subtree rooted at node with value 8. The height of the tree becomes 2 (The path 5 -> 9 -> 3).

**Constraints:**

- The number of nodes in the tree is `n`.
- `2 <= n <= 105`
- `1 <= Node.val <= n`
- All the values in the tree are **unique**.
- `m == queries.length`
- `1 <= m <= min(n, 104)`
- `1 <= queries[i] <= n`
- `queries[i] != root.val`

#### Solution
[Video Explanation](https://youtu.be/eEfW7CLbhvU)

##### Optimal Approach

###### Data Structures:
- `levelMap`: Tracks the level of each node in the tree.
- `heightMap`: Stores the height of each node (distance from the node to the farthest leaf node).
- `levelMaxHt` and `levelSecondMaxHt`: Store the two largest heights for each level. This will help determine the height if we remove a node.

###### Approach

1. **Preprocessing with `preProcess` Function**:
    
    - This function calculates heights and levels of each node.
        
    - **Recursive Traversal**:
        
        - `preProcess` recursively traverses the binary tree, setting levels for each node in `levelMap`.
        - **Level Initialization**:
            - If we encounter a new level (one that isn't yet in `levelMaxHt`), initialize `levelMaxHt` and `levelSecondMaxHt` for this level with zeros.
        - **Height Calculation**:
            - Recursively calculate the heights of left and right subtrees.
            - **Node Height** = `1 + max(left height, right height)`.
            - Store the height in `heightMap`.
    - **Updating Max Heights for Each Level**:
        
        - After calculating a node’s height, check if it’s the highest or second-highest for its level.
        - **If it’s the highest**: Update `levelMaxHt[level]` and shift the previous max height to `levelSecondMaxHt[level]`.
        - **If it’s second-highest**: Update `levelSecondMaxHt[level]`.
2. **Answering Queries with `treeQueries` Function**:
    
    - After preprocessing, iterate through each query to determine the modified height if a node were removed.
    - **Calculate Result for Each Node**:
        - Get the `level` of the queried node.
        - Determine if the node's height is the highest at its level using `heightMap[node] == levelMaxHt[level]`.
            - **If it's the highest**: Use `levelSecondMaxHt[level]` as the alternate height.
            - **Otherwise**: Use `levelMaxHt[level]`.
        - Calculate the result for the node using the formula: result=level+alternate height−1\text{result} = \text{level} + \text{alternate height} - 1result=level+alternate height−1
        - Append this result to the final answer.

*TC ->* O( n + q ) ,where `q` is the number of queries.
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
    unordered_map<int, int> levelMap;
    unordered_map<int, int> heightMap;

    vector<int> levelMaxHt;
    vector<int> levelSecondMaxHt;

    int preProcess(TreeNode* root, int level = 0) {
        if (!root)
            return 0;

        levelMap[root->val] = level;

        if (levelMaxHt.size() == level) {
            levelMaxHt.push_back(0);
            levelSecondMaxHt.push_back(0);
        }

        int left = preProcess(root->left, level + 1);
        int right = preProcess(root->right, level + 1);

        int nodeHeight = 1 + max(left, right);

        if (nodeHeight > levelMaxHt[level]) {
            levelSecondMaxHt[level] = levelMaxHt[level];
            levelMaxHt[level] = nodeHeight;
        } else if (nodeHeight > levelSecondMaxHt[level])
            levelSecondMaxHt[level] = nodeHeight;

        heightMap[root->val] = nodeHeight;

        return nodeHeight;
    }

public:
    vector<int> treeQueries(TreeNode* root, vector<int>& queries) {
        preProcess(root) - 1;
        vector<int> res;

        for (int node : queries) {
            int level = levelMap[node];

            // L + H - 1
            int temp =
                level +
                (heightMap[node] == levelMaxHt[level] ? levelSecondMaxHt[level]
                                                      : levelMaxHt[level]) -
                1;

            res.push_back(temp);
        }
        return res;
    }
};
```