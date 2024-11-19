---
difficulty: easy
leetcode-num: 206
topics:
  - Linked List
  - Recursion
---
[Problem Link](https://leetcode.com/problems/reverse-linked-list/)

#### Problem
Given the `head` of a singly linked list, reverse the list, and return _the reversed list_.

**Example 1:**

![|300](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

**Input:** head = [1,2,3,4,5]
**Output:** [5,4,3,2,1]

**Example 2:**

![img|200](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

**Input:** head = [1,2]
**Output:** [2,1]

**Example 3:**

**Input:** head = []
**Output:** []

**Constraints:**

- The number of nodes in the list is the range `[0, 5000]`.
- `-5000 <= Node.val <= 5000`

#### Solution
[Video Explanation](https://youtu.be/D2vI2DNJGd8)

##### Iterative Approach

We will initialize two pointers before traversing the linked list:
1. `curr`: to point to current node in our traversal ( initially points to `head`)
2. `prev`: to point to the node before `curr`. ( Initially `NULL` coz `curr` is the first node)

We will continue traversing the linked list till `curr` is not `NULL`. In each iteration, we will create a temporary node `next` to point to the node next of `curr`. This is important as we will be making `curr->next` point to `prev` in each iteration.

![[Reverse Linked List (LC206) 2024-07-04 20.23.49.excalidraw|500]]

TC -> `O( n )`
SC -> `O( 1 )`

```cpp title=Code
class Solution {
public:
    ListNode* reverseList(ListNode* head) {

        ListNode* curr = head;
        ListNode* prev = NULL;
        
        while (curr) {
            ListNode* next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }

        return prev;
    }
};
```

##### Recursive Approach

1. **Base Case:**
   - If the list is empty (`head == NULL`) or has only one node (`head->next == NULL`), the function returns the `head` as the reversed list is the same as the original.

2. **Recursive Call:**
   - The function calls itself with the next node (`head->next`) and stores the result in `newHead`. This call continues until the base case is reached.

3. **Reversing the Links:**
   - After the recursive call returns, the function reverses the link between the current node (`head`) and the next node (`front`).
   - It sets `front->next` to point to the current node (`head`), effectively reversing the direction of the link.
   - It sets `head->next` to `NULL` to complete the reversal for the current node.

4. **Return the New Head:**
   - Finally, the function returns `newHead`, which is the new head of the reversed linked list.


![[Reverse Linked List (LC206) 2024-07-04 23.54.25.excalidraw|750]]

TC -> `O( n )`
SC -> `O( n )`

```cpp title=Code
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (head == NULL || head->next == NULL)
            return head;

        ListNode* newHead = reverseList(head->next);
        ListNode* front = head->next;

        front->next = head;
        head->next = NULL;
        return newHead;
    }
};
```

