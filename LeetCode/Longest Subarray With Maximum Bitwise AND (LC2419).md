---
difficulty: medium
leetcode-num: 2419
topics:
  - Array
---
[Problem Link](https://leetcode.com/problems/longest-subarray-with-maximum-bitwise-and/)

#### Problem
You are given an integer array `nums` of size `n`.

Consider a **non-empty** subarray from `nums` that has the **maximum** possible **bitwise AND**.

- In other words, let `k` be the maximum value of the bitwise AND of **any** subarray of `nums`. Then, only subarrays with a bitwise AND equal to `k` should be considered.

Return _the length of the **longest** such subarray_.

The bitwise AND of an array is the bitwise AND of all the numbers in it.

A **subarray** is a contiguous sequence of elements within an array.

**Example 1:**

**Input:** nums = `[1,2,3,3,2,2]`
**Output:** 2
**Explanation:**
The maximum possible bitwise AND of a subarray is 3.
The longest subarray with that value is `[3,3]`, so we return 2.

**Example 2:**

**Input:** nums = [`1,2,3,4]`
**Output:** 1
**Explanation:**
The maximum possible bitwise AND of a subarray is 4.
The longest subarray with that value is `[4]`, so we return 1.

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 106`

#### Solution
##### Optimal Approach
Despite the problem title mentioning "Bitwise AND", this is **not a bit manipulation problem**. We are only concerned with identifying the maximum element and finding the longest streak of this maximum value.

1. **Variables:**    
    - `res`: Stores the result, i.e., the length of the longest subarray where all elements are the maximum.
    - `consCountMax`: Tracks the count of consecutive occurrences of the maximum element.
    - `maxEle`: Stores the maximum element seen so far in the array.
2. **Iteration Over the Array:**    
    - The loop iterates over every element `x` in the array `nums`.
3. **Checking Maximum Element:**    
    - **If `x` equals `maxEle`**: This means the current element matches the maximum element found so far, so we increase `consCountMax` (the count of consecutive occurrences of `maxEle`).
    - **If `x` is greater than `maxEle`**: We have found a new maximum element. Therefore:
        - We reset `consCountMax` to `1` (since this new maximum element is starting its first occurrence).
        - We set `maxEle` to this new value.
        - We also reset `res` to `1`, since the longest subarray of the new maximum value starts at this element.
    - **If `x` is less than `maxEle`**: The current element isn't part of the maximum subarray, so we reset `consCountMax` to `0`.
4. **Updating the Result:**    
    - After each iteration, `res` is updated to keep track of the longest subarray length by comparing it with `consCountMax`.
5. **Final Output:**    
    - Once the loop is done, `res` holds the length of the longest subarray containing the maximum element, and the function returns this value.

*TC ->* O( n )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    int longestSubarray(vector<int>& nums) {
        int res = 0;
        int consCountMax = 0; // consecutive count of max ele
        int maxEle = INT_MIN;

        for (int x : nums) {
            if (maxEle == x) {
                consCountMax++;

            } else if (maxEle < x) {
                consCountMax = 1;
                res = 1;
                maxEle = x;
            } else
                consCountMax = 0;

            res = max(res, consCountMax);
        }

        return res;
    }
};
```