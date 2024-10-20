---
difficulty: easy
leetcode-num: 590
topics:
  - Tree
  - Depth-First Search
---
[Problem Link](https://leetcode.com/problems/n-ary-tree-postorder-traversal/)

#### Problem
Given the `root` of an n-ary tree, return _the postorder traversal of its nodes' values_.

Nary-Tree input serialization is represented in their level order traversal. Each group of children is separated by the null value (See examples)

**Example 1:**

![|250](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

**Input:** root = `[1,null,3,2,4,null,5,6]`
**Output:** `[5,6,3,2,4,1]`

**Example 2:**

![|250](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)

**Input:** root = `[1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]`
**Output:** `[2,6,14,11,7,3,12,8,4,13,9,10,5,1]`

#### Solution

##### Recursive Approach

*TC ->* O( n )
*SC ->* O( h )

```cpp title=Code
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    vector<int> traversal;
public:
    void recursive(Node* root){
        if( root == nullptr)
            return;
        for(auto x: root->children){
            recursive(x);
        }
        traversal.push_back(root->val);
    }
    vector<int> postorder(Node* root) {
        recursive(root);
        return traversal;
    }
};
```