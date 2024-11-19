---
difficulty: medium
leetcode-num: 962
topics:
  - Array
  - Stack
  - Monotonic Stack
  - Sliding Window
---
[Problem Link](https://leetcode.com/problems/maximum-width-ramp/)

#### Problem  
A **ramp** in an integer array `nums` is a pair `(i, j)` for which `i < j` and `nums[i] <= nums[j]`. The **width** of such a ramp is `j - i`.

Given an integer array `nums`, return _the maximum width of a **ramp** in_ `nums`. If there is no **ramp** in `nums`, return `0`.

**Example 1:**

**Input:** `nums = [6,0,8,2,1,5]`
**Output:** 4
**Explanation:** The maximum width ramp is achieved at (i, j) = (1, 5): `nums[1] = 0` and `nums[5] = 5`.

**Example 2:**

**Input:** `nums = [9,8,1,0,1,9,4,0,4,1]`
**Output:** 7
**Explanation:** The maximum width ramp is achieved at (i, j) = (2, 9): `nums[2] = 1` and `nums[9] = 1`.

**Constraints:**

- `2 <= nums.length <= 5 * 104`
- `0 <= nums[i] <= 5 * 104`

#### Solution
##### Optimal Approach (Stack)
#todo
[Video Explanation]()

*TC ->* O(  )
*SC ->* O(  )

```cpp title=Code
```

##### Optimal Approach (Sliding Window)
[Video Explanation](https://youtu.be/3pTEJ1vzgSI)
- **Create `maxRight` Array:**
    
    - The idea is to maintain an array `maxRight` where each element at index `i` represents the maximum element from index `i` to the end of the array.
    - The reason for this is that we want to find the largest possible `j` for a given `i` such that `nums[i] <= nums[j]`.
    
    **How it's built**:
    
    - Initialize `maxRight` as a copy of `nums`.
    - Traverse from right to left (starting from the second last element to the first), and fill `maxRight[i]` with the maximum of `nums[i]` and `maxRight[i + 1]`. This ensures that `maxRight[i]` stores the largest element from index `i` to the end of the array.
- **Find the Maximum Ramp Width:**
    
    - Use two pointers: `left` starting from `0` and `right` iterating over the array from `0` to the end.
    - For each `right`, move `left` forward while `nums[left] > maxRight[right]`. This ensures that you find the smallest possible `i` for the current `j` where `nums[i] <= nums[j]`.
    - For each valid `left` and `right` pair, calculate the width of the ramp (`right - left`) and update `maxWidth` if this width is larger than the current maximum.
- **Return the Maximum Width:**
    
    - After iterating through all possible values of `right`, the algorithm returns the largest ramp width found.

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    int maxWidthRamp(vector<int>& nums) {
        vector<int> maxRight = nums;
        int maxWidth = 0;

        for (int i = nums.size() - 2; i >= 0; i--)
            maxRight[i] = max(nums[i], maxRight[i + 1]);

        int left = 0;

        for (int right = 0; right < nums.size(); right++) {
            while (nums[left] > maxRight[right])
                left++;

            maxWidth = max(maxWidth, right - left);
        }

        return maxWidth;
    }
};
```