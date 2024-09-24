---
difficulty: medium
leetcode-num: 1367
topics:
  - Depth-First Search
  - Linked List
  - Binary Tree
---
[Problem Link](https://leetcode.com/problems/linked-list-in-binary-tree/)

#### Problem
Given a binary tree `root` and a linked list with `head` as the first node. 

Return True if all the elements in the linked list starting from the `head` correspond to some _downward path_ connected in the binary tree otherwise return False.

In this context downward path means a path that starts at some node and goes downwards.

**Example 1:**

**![|250](https://assets.leetcode.com/uploads/2020/02/12/sample_1_1720.png)**

**Input:** head = `[4,2,8]`, root = `[1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]`
**Output:** true
**Explanation:** Nodes in blue form a subpath in the binary Tree.  

**Example 2:**

**![|250](https://assets.leetcode.com/uploads/2020/02/12/sample_2_1720.png)**

**Input:** head = `[1,4,2,6]`, root = `[1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]`
**Output:** true

**Example 3:**

**Input:** head = `[1,4,2,6,8]`, root = `[1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]`
**Output:** false
**Explanation:** There is no path in the binary tree that contains all the elements of the linked list from `head`.

**Constraints:**

- The number of nodes in the tree will be in the range `[1, 2500]`.
- The number of nodes in the list will be in the range `[1, 100]`.
- `1 <= Node.val <= 100` for each node in the linked list and binary tree.

#### Solution
This solution uses recursion to check whether the linked list exists as a path starting from any node in the binary tree.

There are two key parts to the solution:

1. **`check()`**: Verifies whether the current path in the binary tree matches the linked list.
2. **`isSubPath()`**: Recursively traverses the entire tree, looking for a starting point where `check()` can be initiated.

###### 1. **`check(TreeNode* root, ListNode* head)`**:

- **Goal**: This function checks if there exists a matching sub-path starting from the current binary tree node (`root`) and continuing with the linked list node (`head`).
- **Base Cases**:
    - If `root` is `nullptr` (no node) or if the value of `root->val` does not match `head->val`, return `false`. This stops further recursion when a mismatch occurs or when there's no node to match.
    - If `head->next == nullptr`, it means the entire linked list has been successfully matched, so return `true`.
- **Recursive Calls**:
    - If the current node matches the linked list node, we recursively check the left and right child nodes of the current binary tree node (`root->left` and `root->right`) against the next node in the linked list (`head->next`). We return `true` if either the left or right subtree has a matching path (`check(root->left, head->next) || check(root->right, head->next)`).

###### 2. **`isSubPath(ListNode* head, TreeNode* root)`**:

- **Goal**: This function is responsible for finding a starting point for the linked list sub-path by traversing the entire binary tree.
- **Base Case**:
    - If `root == nullptr`, return `false`, as there are no more nodes in the tree to check.
- **Recursive Calls**:
    - First, it calls `check(root, head)` to see if a matching path starts from the current node.
    - If no match is found starting at the current node, it recursively checks the left and right subtrees (`isSubPath(head, root->left)` and `isSubPath(head, root->right)`) to find another possible starting point.

###### How It Works Together:

- The **`isSubPath()`** function traverses the entire binary tree to search for potential starting points for the linked list.
- At each binary tree node, it uses **`check()`** to verify if a sub-path matching the linked list starts at that node.
- If a match is found, it immediately returns `true`. Otherwise, it continues searching in the left and right subtrees.

*TC ->* O( `m * n` )
*SC ->* O( `h + n` )
where h is height of tree, m is number of Tree nodes and n is number of Linked list nodes.

```cpp title=Code
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
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

    bool check(TreeNode* root, ListNode* head) {
        if (!root || root->val != head->val)
            return false;

        if (head->next == nullptr)
            return true;

        return check(root->left, head->next) || check(root->right, head->next);
    }

public:
    bool isSubPath(ListNode* head, TreeNode* root) {
        if (!root)
            return false;

        return check(root, head) || isSubPath(head, root->left) ||
               isSubPath(head, root->right);
    }
};
```