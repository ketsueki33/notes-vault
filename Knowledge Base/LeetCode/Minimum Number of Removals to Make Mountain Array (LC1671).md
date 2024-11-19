---
difficulty: hard
leetcode-num: 1671
topics:
  - Array
  - Dynamic Programming
---
[Problem Link](https://leetcode.com/problems/minimum-number-of-removals-to-make-mountain-array/)

#### Problem
You may recall that an array `arr` is a **mountain array** if and only if:

- `arr.length >= 3`
- There exists some index `i` (**0-indexed**) with `0 < i < arr.length - 1` such that:
    - `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
    - `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

Given an integer array `nums`​​​, return _the **minimum** number of elements to remove to make_ `nums_​​​_` _a **mountain array**._

**Example 1:**

**Input:** nums = [1,3,1]
**Output:** 0
**Explanation:** The array itself is a mountain array so we do not need to remove any elements.

**Example 2:**

**Input:** nums = [2,1,1,5,6,2,3,1]
**Output:** 3
**Explanation:** One solution is to remove the elements at indices 0, 1, and 5, making the array nums = [1,5,6,3,1].

**Constraints:**

- `3 <= nums.length <= 1000`
- `1 <= nums[i] <= 109`
- It is guaranteed that you can make a mountain array out of `nums`.

#### Solution


##### DP Approach
[Video Explanation](https://youtu.be/LFjC2AW0wf4)


> [!info] Longest Increasing Subsequence
> See Bottom Up Approach of [[../DSA/Practice/Dynamic Programming/Longest Increasing Subsequence (LC300)|Longest Increasing Subsequence (LC300)]] to better understand this solution. The same concept is also used to calculate the Longest Decreasing Subsequence in the reverse direction.

###### Key Concepts
1. **LIS (Longest Increasing Subsequence)**: For each element `nums[i]`, calculate the length of the longest increasing sequence ending at that position.
2. **LDS (Longest Decreasing Subsequence)**: For each element `nums[i]`, calculate the longest decreasing sequence starting at that position.
3. **Valid Peak Condition**: A valid peak element should have at least one element on both its left (increasing) and right (decreasing), so `LIS[i] > 1` and `LDS[i] > 1`.

###### Steps in Solution

1. **Initialize Variables**:
    
    - `LIS` and `LDS` vectors, each of size `n` (length of `nums`), initialized to 1 since every element alone forms an increasing or decreasing sequence of length 1.
    - `minRemovals`, set initially to `n`, to track the minimum elements to remove.
2. **Calculate LIS for Each Position**:
    
    - For each element at `i`, check all previous elements `j` (`j < i`). If `nums[i] > nums[j]`, then `nums[i]` can extend the sequence ending at `j`, so update `LIS[i]` as `LIS[i] = max(LIS[i], LIS[j] + 1)`.
3. **Calculate LDS for Each Position**:
    
    - For each element at `i` (starting from the end), check all elements `j` after it (`j > i`). If `nums[i] > nums[j]`, `nums[i]` can extend the decreasing sequence starting at `j`. Update `LDS[i]` as `LDS[i] = max(LDS[i], LDS[j] + 1)`.
4. **Calculate Minimum Removals**:
    
    - For each index `i`, check if `LIS[i] > 1` and `LDS[i] > 1`. If so, `i` can be a valid peak in the mountain array.
    - The length of the mountain sequence at `i` is `LIS[i] + LDS[i] - 1` (we subtract 1 because `i` is counted twice in both sequences).
    - Calculate elements to remove as `n - (LIS[i] + LDS[i] - 1)` and update `minRemovals` if this value is smaller.
5. **Return Result**:
    
    - After iterating through all potential peaks, return `minRemovals` as the minimum number of deletions required to achieve a valid mountain array.


*TC ->* O( n$^{2}$ )
*SC ->* O( n )

```cpp title=Code
class Solution {

public:
    int minimumMountainRemovals(vector<int>& nums) {
        int n = nums.size();

        vector<int> LIS(n, 1);
        vector<int> LDS(n, 1);

        // calculating LIS
        for (int i = 0; i < n; i++)
            for (int j = i - 1; j >= 0; j--)
                if (nums[i] > nums[j])
                    LIS[i] = max(LIS[i], LIS[j] + 1);

        // calculating LDS
        for (int i = n - 1; i >= 0; i--)
            for (int j = i + 1; j < n; j++)
                if (nums[i] > nums[j])
                    LDS[i] = max(LDS[i], LDS[j] + 1);

        int minRemovals = n;

        for (int i = 0; i < n; i++) {
            if (LIS[i] > 1 && LDS[i] > 1)
                minRemovals = min(minRemovals, n - LIS[i] - LDS[i] + 1);
        }

        return minRemovals;
    }
};
```