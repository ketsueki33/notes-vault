---
difficulty: medium
leetcode-num: 3217
topics:
  - Linked List
  - Hash Table
---
[Problem Link](https://leetcode.com/problems/delete-nodes-from-linked-list-present-in-array/)

#### Problem
You are given an array of integers `nums` and the `head` of a linked list. Return the `head` of the modified linked list after **removing** all nodes from the linked list that have a value that exists in `nums`.

**Example 1:**

**Input:** nums = `[1,2,3]`, head = `[1,2,3,4,5]`

**Output:** `[4,5]`

**Explanation:**

**![|400](https://assets.leetcode.com/uploads/2024/06/11/linkedlistexample0.png)**

Remove the nodes with values 1, 2, and 3.

**Example 2:**

**Input:** nums = `[1]`, head = `[1,2,1,2,1,2]`

**Output:** `[2,2,2]`

**Explanation:**

![|420](https://assets.leetcode.com/uploads/2024/06/11/linkedlistexample1.png)

Remove the nodes with value 1.

**Example 3:**

**Input:** nums = `[5]`, head = `[1,2,3,4]
`
**Output:** `[1,2,3,4]`

**Explanation:**

**![|300](https://assets.leetcode.com/uploads/2024/06/11/linkedlistexample2.png)**

No node has value 5.

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 105`
- All elements in `nums` are unique.
- The number of nodes in the given list is in the range `[1, 105]`.
- `1 <= Node.val <= 105`
- The input is generated such that there is at least one node in the linked list that has a value not present in `nums`.

#### Solution
[Video Explanation](https://youtu.be/qb7br7auuOc)

##### Optimal Approach
- **Map Creation (`unordered_map`)**:
    
    - First, it creates a hash map `mp` to store values from the array `nums`. The goal is to mark the values that need to be removed from the list. Each value in `nums` is mapped to `true` in the unordered map (`mp[x] = true`), meaning "this value should be removed if found in the list."
- **Dummy Node**:
    
    - A dummy node is created (`dummy = new ListNode(-1)`) and linked to the head of the list. This dummy node simplifies edge cases where the head node might need to be removed, making the traversal and deletion process cleaner.
- **Traversal and Deletion**:
    
    - The list is traversed using a pointer `curr`, starting from the dummy node. For each node:
        - If the value of the next node (`curr->next->val`) is found in the map (`mp[curr->next->val] == true`), the node needs to be removed.
        - To remove the node, the pointer `curr->next` is adjusted to skip the unwanted node, and the unwanted node is deleted from memory (`delete temp`).
        - If the node's value is not in `mp`, the pointer `curr` moves forward to the next node.
- **Return the Modified List**:
    
    - Once the entire list has been processed, the function returns the modified list starting from `dummy->next`, which points to the new head of the list.

*TC ->* O( n )
*SC ->* O( n )

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
class Solution {
public:
    ListNode* modifiedList(vector<int>& nums, ListNode* head) {
        unordered_map<int, bool> mp;

        for (int x : nums)
            mp[x] = true;

        ListNode* dummy = new ListNode(-1);
        dummy->next = head;

        ListNode* curr = dummy;

        while (curr->next != nullptr) {

            if (mp[curr->next->val] == true) {
                ListNode* temp = curr->next;
                curr->next = curr->next->next;
                delete (temp);
            } else
                curr = curr->next;
        }

        return dummy->next;
    }
};
```