---
difficulty: easy
leetcode-num: 101
topics:
  - Binary Tree
  - Depth-First Search
  - Breadth-First Search
---
[Problem Link]()

#### Problem


#### Solution
[Video Explanation]()

##### Iterative (BFS) Approach
- **Initialization**:
    
    - The function starts by checking the `root`. If there is a `root`, it pushes its left and right children into a queue.
    - The queue is used to process nodes level by level in pairs, ensuring that the nodes being compared are from opposite sides of the tree.
- **Main Loop**:
    
    - The algorithm enters a `while` loop that continues as long as the queue isn't empty. Each iteration of the loop processes two nodes, `rootL` and `rootR` (one from the left subtree and one from the right subtree).
- **Null Checks**:
    
    - The algorithm first checks if both `rootL` and `rootR` are `nullptr`. If both are `nullptr`, it means the tree is symmetric at this level, and it continues to the next pair of nodes.
    - If **one node is `nullptr` and the other is not**, or if **their values don't match**, the function returns `false`, as this indicates asymmetry.
- **Enqueuing Children**:
    
    - If the current nodes are valid and their values match, the function adds their children to the queue:
        - `rootL->left` is paired with `rootR->right` (to check symmetry on the outer sides).
        - `rootL->right` is paired with `rootR->left` (to check symmetry on the inner sides).
    - This ensures that symmetry is checked level by level, with each pair of nodes coming from opposite sides of the tree.
- **Conclusion**:
    
    - If all node pairs are checked without finding any asymmetry, the function returns `true`, indicating the tree is symmetric.

*TC ->* O( n )
*SC ->* O( w ), where w is max width of the tree

```cpp title=Code
class Solution {

public:
    bool isSymmetric(TreeNode* root) {
        queue<TreeNode*> q;

        if (root) {
            q.push(root->left);
            q.push(root->right);
        }

        while (!q.empty()) {
            TreeNode* rootL = q.front();
            q.pop();
            TreeNode* rootR = q.front();
            q.pop();

            if (rootL == nullptr && rootR == nullptr)
                continue;

            if (rootL == nullptr || rootR == nullptr ||
                rootL->val != rootR->val)
                return false;

            q.push(rootL->left);
            q.push(rootR->right);

            q.push(rootL->right);
            q.push(rootR->left);
        }
        return true;
    }
};
```

##### Recursive (DFS) Approach
1. **Main Function: `isSymmetric(TreeNode* root)`**

- If the tree is empty (`root == nullptr`), it is symmetric by definition, so the function returns `true`.
- If the tree is not empty, it calls the helper function `checkSymmetry` to compare the left and right subtrees.

2. **Helper Function: `checkSymmetry(TreeNode* root1, TreeNode* root2)`**
This function takes two tree nodes (`root1` and `root2`) and checks whether they form a mirror image of each other.
- **Base Cases:**
	
	- If both `root1` and `root2` are `nullptr`, it means we've reached the end of both subtrees, and they are symmetric up to this point. The function returns `true`.
	- If one of `root1` or `root2` is `nullptr` and the other is not, they are not symmetric, so the function returns `false`.
- **Recursive Case:**
	
	- If `root1` and `root2` have the same value, we recursively check:
		- `root1->left` with `root2->right` (mirror check).
		- `root1->right` with `root2->left` (mirror check).
	- All these conditions must hold for the trees to be symmetric.

3. **Return Value:**
The result of `checkSymmetry` determines whether the tree is symmetric:

- If `checkSymmetry(root->left, root->right)` returns `true`, the entire tree is symmetric.
- If not, the tree is asymmetric, and the function returns `false`.

*TC ->* O( n )
*SC ->* O( h ), where h is height (recursion depth)

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
    bool checkSymmetry(TreeNode* root1, TreeNode* root2) {
        if (root1 == nullptr && root2 == nullptr)
            return true;

        if (root1 == nullptr || root2 == nullptr)
            return false;

        return (root1->val == root2->val) &&
               (checkSymmetry(root1->left, root2->right)) &&
               (checkSymmetry(root1->right, root2->left));
    }

public:
    bool isSymmetric(TreeNode* root) {
        if (!root)
            return true;
        return checkSymmetry(root->left, root->right);
    }
};
```