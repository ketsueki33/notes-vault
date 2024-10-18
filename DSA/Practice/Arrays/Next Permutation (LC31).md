---
difficulty: medium
leetcode-num: 31
topics:
  - Array
  - Two Pointer
---
[Problem Link](https://leetcode.com/problems/next-permutation/)

#### Problem
A **permutation** of an array of integers is an arrangement of its members into a sequence or linear order.

- For example, for `arr = [1,2,3]`, the following are all the permutations of `arr`: `[1,2,3], [1,3,2], [2, 1, 3], [2, 3, 1], [3,1,2], [3,2,1]`.

The **next permutation** of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the **next permutation** of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).

- For example, the next permutation of `arr = [1,2,3]` is `[1,3,2]`.
- Similarly, the next permutation of `arr = [2,3,1]` is `[3,1,2]`.
- While the next permutation of `arr = [3,2,1]` is `[1,2,3]` because `[3,2,1]` does not have a lexicographical larger rearrangement.

Given an array of integers `nums`, _find the next permutation of_ `nums`.

The replacement must be **[in place](http://en.wikipedia.org/wiki/In-place_algorithm)** and use only constant extra memory.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:** [1,3,2]

**Example 2:**

**Input:** nums = [3,2,1]
**Output:** [1,2,3]

**Example 3:**

**Input:** nums = [1,1,5]
**Output:** [1,5,1]

**Constraints:**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`

#### Solution
[Video Explanation](https://youtu.be/JDOXKqF60RQ)

##### Optimal Approach
The solution consists of 4 steps:

**Step 1: Finding the first 'low' index**:

- The algorithm begins by identifying the first position from the end where a smaller element exists before a larger element (`nums[low] < nums[low + 1]`).
- This means that from index `low + 1` to the end, the sequence is already **non-increasing** or **descending**. This is because, after `low`, there are no smaller-to-larger transitions; it's a strictly decreasing sequence, which represents the largest possible permutation of that part of the array.

**Step 2: Handling edge case**:

- If no such `low` is found (i.e., `low < 0`), this indicates that the array is entirely in descending order, which means it's the **last permutation**. In this case, reversing the entire array gives the **smallest permutation**.

**Step 3: Swapping**:    
 
- The next step is to find the smallest element in the suffix (i.e., the portion after `low`) that is **greater than** `nums[low]`. This ensures that when you swap `nums[low]` with this element, you're still maintaining the lexicographical order but creating the next permutation.
- After swapping, the part after `low` still remains in descending order because swapping does not disturb the non-increasing order of elements after `low`.

**Step 4: Reversing the suffix**:   
- After the swap, the part after `low` is still in **descending order**.
- To get the **next permutation**, we need the smallest possible sequence after `low`. Since the part after `low` is in descending order, reversing it will give the **smallest possible order** (i.e., ascending), which is needed to create the next lexicographically greater permutation.


> [!NOTE] Why is the suffix in descending order?
> - The suffix (after `low`) was **already descending** before the swap (as found in step 1).
> -  Swapping `nums[low]` with the smallest number greater than it does not change the descending order of the rest of the elements after `low`. The only thing that changes is the element at `low` itself.
> - The reason the part after `low` is in descending order is that we specifically identify the position (`low`) where the array transitions from an increasing sequence to a decreasing one. Thus, everything after `low` is the largest possible permutation of that part of the array. Reversing this part gives the smallest permutation, allowing us to form the next permutation in lexicographical order.




*TC ->* O( n )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int n= nums.size(), low;
        
        // Step 1: Find the first `low` index
        for (low = n - 2; low >= 0; low--) {
            if (nums[low] < nums[low + 1])
                break;
        }
        
        // Step 2: Handle Edge Case
        if (low < 0) {
            reverse(nums.begin(), nums.end());
            return;
        }

        // Step 3: Swapping
        for (int high = n - 1; high > low; high--) {
            if (nums[high] > nums[low]) {
                swap(nums[high], nums[low]);
                break;
            }
        }

        // Step 4: Reversing the Suffix
        reverse(nums.begin() + low + 1, nums.end());
    }
};

```

#### Similar Problems
- [[../Maths/Next Greater Element III (LC556)|Next Greater Element III (LC556)]]