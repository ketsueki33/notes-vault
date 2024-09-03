#### Trees
**Trees are a non-linear data structure used to represent a hierarchy.**

A Tree is a collection of nodes connected by directed (or undirected) edges.

##### Terminology
###### Degree
Degree of a node is the number of children that node has.

Degree of a Tree is the highest degree of all the nodes in that tree.

###### Internal Node
An internal node (also known as an inner node, inode for short, or branch node) is any node of a tree that has child nodes.

###### External Node
An external node (also known as an outer node, leaf node, or terminal node) is any node that does not have child nodes.

#### Binary Trees
Binary Trees are trees in which the degree of a node can be 0, 1 or 2 i.e a node can have at most 2 children.

![[../_assets/Binary Trees 2024-08-31 20.00.36.excalidraw.png|450]]

**implementation of Binary Tree Node**
```cpp
struct Node {
    int val;
    Node *left, *right;

    Node(int x) {
        val = x;
        right = left = nullptr;
    }
};
```

#### Types of Binary Tree
##### 1. Full Binary Tree
A full binary tree is a binary tree where every node has either 0 or 2 children. No node can have only 1 child. This type of tree is also known as a strict binary tree. 

![[../_assets/Pasted image 20240831201426.png|500]]

##### 2. Perfect Binary Tree
A perfect binary tree is a full binary tree where all leaf nodes are at the same depth and all internal nodes have two children. The height of a perfect binary tree is log$_{2}$n, where n is the number of nodes in the tree.

![[../_assets/Pasted image 20240831201717.png|500]]
##### 3. Complete Binary Tree
A complete binary tree is a binary tree where every level is completely filled, except for possibly the last level, which is filled from left to right. This type of tree is often used in algorithms that require data to be stored in a heap structure, such as a priority queue or heap sort. The advantage of using a complete binary tree over other types of trees is that the tree can be efficiently stored in an array.

![[../_assets/Pasted image 20240831201911.png|520]]
##### 4. Degenerate Binary Tree
A degenerate binary tree is a tree in which every internal node has only one child. This results in a tree that is similar to a linked list.

![[../_assets/Pasted image 20240831202006.png|520]]

##### 5. Balanced Binary Tree
A balanced binary tree is a binary tree where the height difference between the left and right subtrees of any node is no greater than some constant value. This helps ensure fast search times and prevent the tree from becoming too unbalanced. Examples of balanced binary trees include red-black trees and AVL trees. 

![[../_assets/Pasted image 20240831202140.png|520]]

#### Tree Traversal 
##### Depth First Search
###### Inorder Traversal
The order of traversal is:
*Left -> Root -> Right*

Which means the Left subtree will be printed before the current node, after which the right subtree will be printed.

![[../_assets/Pasted image 20240831234122.png|400]]

**Recursive implementation:**
```cpp
void inorder(TreeNode* root){
	if( !root ) return;

	inorder(root->left);
	cout<<root->val;
	inorder(root->right);
}
```
[[../Practice/Binary Trees/Binary Tree Inorder Traversal (LC94)#Iterative Approach|see here]] for iterative implementation
###### Preorder Traversal
The order of traversal is:
*Root -> Left -> Right*

![[../_assets/Pasted image 20240831234701.png|400]]

**Recursive implementation:**
```cpp
void preorder(TreeNode* root) {
	if (!root)
		return;

	cout<<root->val;
	preorder(root->left);
	preorder(root->right);
}
```
[[../Practice/Binary Trees/Binary Tree Preorder Traversal (LC144)#Iterative Approach|see here]] for iterative implementation
###### Postorder Traversal
The order of traversal is:
*Left -> Right -> Root*

![[../_assets/Pasted image 20240831235011.png|400]]

**Recursive implementation:**
```cpp
void postorder(TreeNode* root) {
	if (!root)
		return;

	postorder(root->left);
	postorder(root->right);
	cout<<root->val;
}
```
[[../Practice/Binary Trees/Binary Tree Postorder Traversal (LC145)#Iterative Approach|see here]] for iterative approach
##### Breadth First Traversal (Level Order)
Each level of the tree is traversed from *Left* to *Right* starting from the first level.

![[../_assets/Pasted image 20240831235349.png|400]]

**Implementation:**
```cpp
void levelOrder(TreeNode *root) {
    queue<TreeNode *> q;
    q.push(root);

    while (!q.empty()) {
        TreeNode *curr = q.front();
        q.pop();
        cout << root->val;

        if (curr->left)
            q.push(curr->left);

        if (curr->right)
            q.push(curr->right);
    }
}
```

**If we want to print each level separately:**
```cpp
void lineByLine(TreeNode *root) {
    queue<TreeNode *> q;
    q.push(root);

    while (!q.empty()) {
        int levelSize = q.size();
        for (int i = 0; i < levelSize; i++) {
            TreeNode *curr = q.front();
            q.pop();
            
            cout << root->val;

            if (curr->left)
                q.push(curr->left);

            if (curr->right)
                q.push(curr->right);
        }
    }
}
```

#### Height of a Binary Tree
Height of a Binary Tree is the number of nodes in the longest path between the root and any of the leaf nodes.
```cpp
int height(TreeNode *root) {
    if (!root)
        return 0;

    return max(height(root->left), height(root->right)) + 1;
}
```

#### Constructing a Tree
A Binary Tree can only be constructed if we have:
1. Inorder and Preorder traversal of the tree
2. Inorder and Postorder traversal of thee tree.

###### From Inorder and Postorder
We will use the Preorder traversal to determine the root of the tree. The  root always appears first in the Preorder.

After we find the root, we will use the Inorder traversal to determine which nodes lie on the left and right subtrees of the node.

We can then break the problem into a sub-problem and construct the tree recursively.

**Important thing** to note how `preIndex` is used to get the root of the current subtree every time.
Since we will recursively call `createTree` for left subtree first, `preorder[preIndex]` will give the root of the current subtree if we increment `preIndex` by 1 each time.
```cpp
class Tree {
    int preIndex = 0;

    TreeNode* createTree(vector<int>& inorder, int in_start, int in_end,
                         vector<int>& preorder) {
        if (in_end < in_start)
            return nullptr;

        int rootVal = preorder[preIndex++];
        int rootIndex;

        for (int i = in_start; i <= in_end; i++) {
            if (rootVal == inorder[i]) {
                rootIndex = i;
                break;
            }
        }

        TreeNode* root = new TreeNode(rootVal);

        root->left = createTree(inorder, in_start, rootIndex - 1, preorder);
        root->right = createTree(inorder, rootIndex + 1, in_end, preorder);

        return root;
    }

public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return createTree(inorder, 0, preorder.size() - 1, preorder);
    }
};
```

The time complexity of this algorithm is O( n$^{2}$ ).. we can reduce it to O( n ) if we use a hash map to find `rootIndex` directly. [[../Practice/Binary Trees/Construct Binary Tree from Preorder and Inorder Traversal (LC105)#Optimal Approach|(see here)]]

###### From Inorder and Postorder
We will use the Postorder traversal to determine the root of the tree. The  root always appears last in the postorder traversal.

After we find the root, we will use the Inorder traversal to determine which nodes lie on the left and right subtrees of the node.

We can then break the problem into a sub-problem and construct the tree recursively.

**Important thing** to note how `postIndex` is used to get the root of the current subtree every time.
Since we will recursively call `createTree` for right subtree first, `postorder[postIndex]` will give the root of the current subtree if we decrement `postIndex` by 1 each time.
```cpp
class Tree {
    int postIndex;

    TreeNode* createTree(vector<int>& inorder, int in_start, int in_end,
                         vector<int>& postorder) {
        if (in_end < in_start)
            return nullptr;

        int rootVal = postorder[postIndex--];
        int rootIndex;

        for (int i = in_start; i <= in_end; i++) {
            if (rootVal == inorder[i]) {
                rootIndex = i;
                break;
            }
        }

        TreeNode* root = new TreeNode(rootVal);

        root->right = createTree(inorder, rootIndex + 1, in_end, postorder);
        root->left = createTree(inorder, in_start, rootIndex - 1, postorder);

        return root;
    }

public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        postIndex = postorder.size() - 1;

        return createTree(inorder, 0, postorder.size() - 1, postorder);
    }
};
```

The time complexity of this algorithm is O( n$^{2}$ ).. we can reduce it to O( n ) if we use a hash map to find `rootIndex` directly. [[../Practice/Binary Trees/Construct Binary Tree from Inorder and Postorder Traversal (LC106)#Optimal Approach|(see here)]]

#### Least Common Ancestor of a Binary Tree
The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in the tree that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).

**Algorithm to find Least Common Ancestor**
We will use recursion while keeping the following things in mind:
- If `root` is same as `p` or `q` ... return `root`.
- Check for `p` and `q` in left and right subtrees.
	- If one subtree contains `p` and other contains `q` then return `root`.
	- If one subtree contains both `p` and `q`.. then return the result of that subtree.
	- If neither contain `p` nor `q`.. return `nullptr`

```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
	if (root == nullptr || root == p || root == q)
		return root;

	TreeNode* left = lowestCommonAncestor(root->left, p, q);
	TreeNode* right = lowestCommonAncestor(root->right, p, q);

	if (left && right)
		return root;

	if (left)
		return left;

	if (right)
		return right;

	return nullptr;
}
```

