---
difficulty: medium
leetcode-num: 2584
topics:
  - Binary Tree
  - Breadth-First Search
  - Heap
---
[Problem Link](https://leetcode.com/problems/kth-largest-sum-in-a-binary-tree/)

#### Problem
You are given the `root` of a binary tree and a positive integer `k`.

The **level sum** in the tree is the sum of the values of the nodes that are on the **same** level.

Return _the_ `kth` _**largest** level sum in the tree (not necessarily distinct)_. If there are fewer than `k` levels in the tree, return `-1`.

**Note** that two nodes are on the same level if they have the same distance from the root.

**Example 1:**

![|200](https://assets.leetcode.com/uploads/2022/12/14/binaryytreeedrawio-2.png)

**Input:** root = [5,8,9,2,1,3,7,4,6], k = 2
**Output:** 13
**Explanation:** The level sums are the following:
- Level 1: 5.
- Level 2: 8 + 9 = 17.
- Level 3: 2 + 1 + 3 + 7 = 13.
- Level 4: 4 + 6 = 10.
The 2nd largest level sum is 13.

**Example 2:**

![|200](https://assets.leetcode.com/uploads/2022/12/14/treedrawio-3.png)

**Input:** root = [1,2,null,3], k = 1
**Output:** 3
**Explanation:** The largest level sum is 3.

**Constraints:**

- The number of nodes in the tree is `n`.
- `2 <= n <= 105`
- `1 <= Node.val <= 106`
- `1 <= k <= n`

#### Solution
##### Optimal Approach
- **Initialize Data Structures:**
    
    - `priority_queue<long long, vector<long long>, greater<long long>> pq;`: This min-heap will store the k largest sums of the tree levels. It uses `greater<long long>` to ensure the smallest sum is always at the top.
    - `queue<TreeNode*> q;`: This queue helps in performing BFS, which ensures we traverse level by level.
- **BFS Traversal (Level-by-Level):**
    
    - We begin by adding the root node to the queue. For each level of the tree, we calculate the sum of all node values at that level (`levelSum`).
    - For each node, we add its value to `levelSum`. We also enqueue its left and right children (if they exist) to process them in the subsequent level.
- **Maintaining the k Largest Level Sums:**
    
    - After calculating the sum for each level, we push `levelSum` to the min-heap (`pq`).
    - The key part is that we **only keep the k largest sums** in the heap. If the size of the heap exceeds k, we pop the smallest sum (using `pq.pop()`).
    - This ensures that when we finish processing all the levels, the heap will contain exactly the k largest sums, with the smallest of these sums at the top of the heap.
- **Handling Edge Cases:**
    
    - If the tree has fewer than k levels, the final size of the heap will be smaller than k, and we return `-1`, indicating that the k-th largest sum does not exist.
- **Return Result:**
    
    - The k-th largest sum is the top element of the min-heap (`pq.top()`), since we ensured the heap contains the k largest sums and the smallest among them is at the top.

*TC ->* O( `n * log k` )
*SC ->* O( w + k ) where w is max width of the tree, and k is the size of the heap.

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
    long long kthLargestLevelSum(TreeNode* root, int k) {
        priority_queue<long long, vector<long long>, greater<long long>> pq;
        queue<TreeNode*> q;

        if (root)
            q.push(root);

        while (!q.empty()) {
            int levelSize = q.size();
            long long levelSum = 0;

            for (int i = 0; i < levelSize; i++) {
                TreeNode* curr = q.front();
                q.pop();

                levelSum += curr->val;

                if (curr->left)
                    q.push(curr->left);

                if (curr->right)
                    q.push(curr->right);
            }

            pq.push(levelSum);
            if (pq.size() > k)
                pq.pop();
        }

        if (pq.size() < k)
            return -1;

        return pq.top();
    }
};
```