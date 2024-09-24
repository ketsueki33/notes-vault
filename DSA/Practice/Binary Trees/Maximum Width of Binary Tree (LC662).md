---
difficulty: medium
leetcode-num: 662
topics:
  - Breadth-First Search
  - Binary Tree
---
[Problem Link](https://leetcode.com/problems/maximum-width-of-binary-tree/)

#### Problem


#### Solution
##### Optimal BFS Approach
This is an improvement over the [[#BFS Approach(using unsigned long long to handle int overflow)|approach below]] so see it first. 

We don't have to include null nodes left a non-null node in our `maxWidth`. Therefore we can adjust the `xIndex` so that the first non-null gets the `xIndex` as `1`.

![[../../_assets/Pasted image 20240904004715.png|550]]

We are still using **long** even though `maxWidth` will be `int` because when calculating `xIndex * 2`.. it will overflow  in `int`.

*TC ->* O( n )
*SC ->* O( w ) , where w is max width of tree

```cpp title=Code
class Solution {
    typedef pair<TreeNode*, long> P;

public:
    int widthOfBinaryTree(TreeNode* root) {
        queue<P> q;
        q.push({root, 1});

        long maxWidth = 0;

        while (!q.empty()) {
            int levelSize = q.size();
            long start = q.front().second;
            long end = q.back().second;

            maxWidth = max(maxWidth, end - start + 1);
            for (int i = 0; i < levelSize; i++) {
                TreeNode* curr = q.front().first;
                long xIndex = q.front().second - start + 1;
                q.pop();

                if (curr->left)
                    q.push({curr->left, (xIndex - 1) * 2 + 1});

                if (curr->right)
                    q.push({curr->right, (xIndex - 1) * 2 + 2});
            }
        }
        return maxWidth;
    }
};
```

##### BFS Approach( using unsigned long long to handle int overflow)
Since we have to count null nodes between two non-null nodes in this problem.. we will assign `xIndex` in such a way that also takes into account the null nodes. We will count the nodes on the left side of the current node's left child as if it were a complete tree and then:
- `xIndex` of `curr->left` = `(nodes on left of left child) + 1`
- `xIndex` of `curr->right` = `(nodes on left of left child) + 2`

`(nodes on left of left child)` will be `(xIndex-1) * 2` as there will be `xIndex - 1` nodes on the left of current node and each of those nodes will have `2` children.

![[../../_assets/Pasted image 20240904003325.png|550]]

We will calculate width of each level as `end - start + 1` , and if it's greater than `maxWidth` we will update `maxWidth`.

**We have used unsigned long long** to handle `int` overflow as after 32nd level.. the value of `xIndex` will cross `int` ( 32 bits ).


*TC ->* O( n )
*SC ->* O( w ) , where w is max width of tree

```cpp title=Code
class Solution {
    typedef pair<TreeNode*, unsigned long long> P;

public:
    int widthOfBinaryTree(TreeNode* root) {
        queue<P> q;
        q.push({root, 1});

        unsigned long long maxWidth = 0;

        while (!q.empty()) {
            int levelSize = q.size();
            unsigned long long start = q.front().second;
            unsigned long long end = q.back().second;

            maxWidth = max(maxWidth, end - start + 1);
            for (int i = 0; i < levelSize; i++) {
                TreeNode* curr = q.front().first;
                unsigned long long xIndex = q.front().second;
                q.pop();

                if (curr->left)
                    q.push({curr->left, (xIndex - 1) * 2 + 1});

                if (curr->right)
                    q.push({curr->right, (xIndex - 1) * 2 + 2});
            }
        }
        return maxWidth;
    }
};
```