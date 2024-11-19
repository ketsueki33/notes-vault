---
difficulty: medium
leetcode-num: 725
topics:
  - Linked List
---
[Problem Link](https://leetcode.com/problems/split-linked-list-in-parts/)

#### Problem
Given the `head` of a singly linked list and an integer `k`, split the linked list into `k` consecutive linked list parts.

The length of each part should be as equal as possible: no two parts should have a size differing by more than one. This may lead to some parts being null.

The parts should be in the order of occurrence in the input list, and parts occurring earlier should always have a size greater than or equal to parts occurring later.

Return _an array of the_ `k` _parts_.

**Example 1:**

![|200](https://assets.leetcode.com/uploads/2021/06/13/split1-lc.jpg)

**Input:** head = `[1,2,3]`, k = 5
**Output:** `[[1],[2],[3],[],[]]`
**Explanation:**
The first element `output[0]` has `output[0].val = 1`, `output[0].next = null`.
The last element `output[4]` is null, but its string representation as a ListNode is `[]`.

**Example 2:**

![|550](https://assets.leetcode.com/uploads/2021/06/13/split2-lc.jpg)

**Input:** head = `[1,2,3,4,5,6,7,8,9,10]`, k = 3
**Output:** `[[1,2,3,4],[5,6,7],[8,9,10]]`
**Explanation:**
The input has been split into consecutive parts with size difference at most 1, and earlier parts are a larger size than the later parts.

**Constraints:**

- The number of nodes in the list is in the range `[0, 1000]`.
- `0 <= Node.val <= 1000`
- `1 <= k <= 50`

#### Solution
- **Calculate Total Length:**
    - Traverse the list once to count the total number of nodes.
- **Determine Group Sizes:**
    - Divide the total node count by `k` to determine how many nodes each part should have.
    - Calculate how many parts need one extra node using the remainder from the division.
- **Split the List:**
    - Traverse the list again to form the parts.
    - For each part, assign the calculated number of nodes. If there are leftover nodes, add one extra node to some parts.
- **Break Links Between Parts:**
    - Ensure that the last node of each part points to `nullptr`, breaking the link to the next part of the list.
- **Handle Extra Parts:**
    - If the list has fewer nodes than `k`, some parts will be `nullptr`.

*TC ->* O( n )
*SC ->* O( 1 )

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
    vector<ListNode*> splitListToParts(ListNode* head, int k) {
        vector<ListNode*> res;
        int count = 0;

        ListNode* temp = head;

        while (temp != nullptr) {
            count++;
            temp = temp->next;
        }

        int groupSize = count / k;
        int extra = count % k;

        temp = head;
        ListNode* prev = nullptr;

        while (k--) {
            if (prev)
                prev->next = nullptr;

            res.push_back(temp);

            if (temp == nullptr)
                continue;

            for (int i = 0; i < groupSize; i++) {
                prev = temp;
                temp = temp->next;
            }

            if (extra > 0) {
                prev = temp;
                temp = temp->next;
                extra--;
            }
        }
        return res;
    }
};
```
