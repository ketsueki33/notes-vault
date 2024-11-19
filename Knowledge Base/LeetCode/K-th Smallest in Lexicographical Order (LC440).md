---
difficulty: hard
leetcode-num: 440
topics:
  - Trie
---
[Problem Link](https://leetcode.com/problems/k-th-smallest-in-lexicographical-order/)

#### Problem
Given two integers `n` and `k`, return _the_ `kth` _lexicographically smallest integer in the range_ `[1, n]`.

**Example 1:**

**Input:** n = 13, k = 2
**Output:** 10
**Explanation:** The lexicographical order is `[1, 10, 11, 12, 13, 2, 3, 4, 5, 6, 7, 8, 9]`, so the second smallest number is 10.

**Example 2:**

**Input:** n = 1, k = 1
**Output:** 1

**Constraints:**

- `1 <= k <= n <= 109`

#### Solution
[Video Explanation](https://youtu.be/pQ_BQ9J9p-c)

##### Optimal Approach
> [!info] Complex Solution
> Watching video helps

1. **The `findCount` Function**:

- This function counts how many numbers exist in the tree under a specific prefix.
- It does this by looking at numbers starting with `curr` (the current prefix) and stopping before `next` (the next prefix).
- It iterates over the range of numbers under the prefix and adds them to `countNum`. If the `curr` grows too large (i.e., goes beyond the `limit`), the function stops counting.
- The function handles multiple levels of prefixes by multiplying `curr` and `next` by `10`, effectively moving deeper into the tree (e.g., from `1` to `10` and from `10` to `100`).

 2. **The `findKthNumber` Function**:

- This is the main function that uses the `findCount` helper to navigate the prefix tree and find the `k-th` smallest number.
- **Initialization**:
    - The current number `curr` is initialized to `1`, and `k` is decremented by `1` because we are already counting the first number.
- **Main Loop**:
    - The function repeatedly calls `findCount` to determine how many numbers are within the subtree under the current prefix (`curr`).
    - **Two Cases**:
        1. **If `count <= k`**:
            - This means that the entire subtree under the current prefix can be skipped. We subtract `count` from `k` and move to the next prefix (`curr++`).
        2. **If `count > k`**:
            - This means that the k-th number is somewhere within this subtree, so instead of skipping the subtree, we go deeper into the tree by multiplying `curr` by `10`.
    - This process continues until `k` reaches `0`, at which point `curr` holds the k-th smallest number.

---
*Example to Understand:*
Suppose `limit = 20` and `k = 2`:

- The lexicographical order of numbers from 1 to 20 is: `1, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 2, 20, 3, 4, ..., 9`.
- We start with `curr = 1` and `k = 1` (since we already counted `1` as the first number).
- The `findCount` function tells us that there are 11 numbers under the prefix `1` (`1, 10, 11, 12, ..., 19`).
    - Since 11 > 1, we go deeper into the tree under `1`. We now explore `curr = 10` and decrement `k = 0`.
    - As soon as `k = 0`, we return `curr = 10` as the 2nd smallest number.
---
*TC ->* O( log$_{10}$ n )
*SC ->* O( log$_{10}$ n ), if we take recursion stack space into account.. otherwise 1.

```cpp title=Code
class Solution {
    int findCount(long curr, long next, int limit) {
        int countNum = 0;

        while (curr <= limit) {
            countNum += next - curr;

            curr *= 10;
            next *= 10;

            next = min(next, long(limit + 1));
        }

        return countNum;
    }

public:
    int findKthNumber(int limit, int k) {
        long curr = 1;
        k--; // since we start from first number, we need k-1 more numbers

        while (k > 0) {
            int count = findCount(curr, curr + 1, limit);

            if (count <= k) {
                k = k - count; // skipping elements under current prefix tree
                curr++;
            } else {
                curr *= 10; // go deeper into current tree
                k--;
            }
        }

        return curr;
    }
};
```