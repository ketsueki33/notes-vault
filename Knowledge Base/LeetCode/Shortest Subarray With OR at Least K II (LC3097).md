---
difficulty: medium
leetcode-num: 3097
topics:
  - Array
  - Bit Manipulation
  - Sliding Window
---
[Problem Link](https://leetcode.com/problems/shortest-subarray-with-or-at-least-k-ii/)

#### Problem
You are given an array `nums` of **non-negative** integers and an integer `k`.

An array is called **special** if the bitwise `OR` of all of its elements is **at least** `k`.

Return _the length of the **shortest** **special** **non-empty**_ _subarray_ _of_ `nums`, _or return_ `-1` _if no special subarray exists_.

**Example 1:**

**Input:** nums = [1,2,3], k = 2

**Output:** 1

**Explanation:**

The subarray `[3]` has `OR` value of `3`. Hence, we return `1`.

**Example 2:**

**Input:** nums = [2,1,8], k = 10

**Output:** 3

**Explanation:**

The subarray `[2,1,8]` has `OR` value of `11`. Hence, we return `3`.

**Example 3:**

**Input:** nums = [1,2], k = 0

**Output:** 1

**Explanation:**

The subarray `[1]` has `OR` value of `1`. Hence, we return `1`.

**Constraints:**

- `1 <= nums.length <= 2 * 105`
- `0 <= nums[i] <= 109`
- `0 <= k <= 109`

#### Solution
[Video Explanation](https://youtu.be/pXr8CF7-5_Y)

##### Optimal Approach
###### Key Functions Explained

1. **`addOR(int x)`**:
    
    - Adds the bitwise `OR` representation of the number `x` into `bitCount`.
    - Iterates through each bit position in `x`. If a bit at a position `pos` is `1`, it increments `bitCount[pos]` to track the count of `1`s in that position for the current subarray.
2. **`removeOR(int x)`**:
    
    - Removes the bitwise `OR` representation of the number `x` from `bitCount`.
    - Similar to `addOR`, it iterates through each bit position in `x`. If a bit at position `pos` is `1`, it decrements `bitCount[pos]` to update the count of `1`s as elements leave the subarray.
3. **`extractOR()`**:
    
    - Constructs the current `OR` result based on the `bitCount` map.
    - For each bit position where `count` is greater than `0` in `bitCount`, the corresponding bit is set to `1` in the result (since at least one number in the subarray has a `1` in that position).
    - Returns the total OR value of the current subarray.

###### Main Function: 
1. **Sliding Window Setup**:
    
    - Uses two pointers `i` and `j` to create a sliding window across `nums`. `j` expands the window by including new elements on the right, and `i` contracts the window from the left to find the minimum subarray length.
2. **Expanding the Window (`addOR(nums[j])`)**:
    
    - For each `j`, it calls `addOR` to add `nums[j]` to the current subarray, updating the `bitCount` with `nums[j]`'s bitwise values.
3. **Contracting the Window**:
    
    - Checks if the current OR result (obtained from `extractOR`) is greater than or equal to `k`.
    - If so, updates `res` to the minimum length (`j - i + 1`).
    - Then, removes `nums[i]` from the subarray by calling `removeOR`, and increments `i` to try finding a smaller valid subarray.
4. **Result Check**:
    
    - If no valid subarray was found, `res` remains `INT_MAX`, and the function returns `-1`. Otherwise, it returns the smallest length found.

*TC ->* O( `32* n` ), as max number of bits in an `int` is 32.
*SC ->* O( `32` )

```cpp title=Code
class Solution {
    unordered_map<int, int> bitCount;

    void addOR(int x) {
        int pos = 0;

        while (x) {
            if (x & 1 == 1)
                bitCount[pos]++;
            pos++;
            x >>= 1;
        }
    }

    void removeOR(int x) {
        int pos = 0;

        while (x) {
            if ((x & 1) == 1)
                bitCount[pos]--;
            pos++;
            x >>= 1;
        }
    }

    int extractOR() {
        int OR = 0;

        for (auto& [pos, count] : bitCount) {
            if (count == 0)
                continue;
            OR += (1 << pos);
        }
        return OR;
    }

public:
    int minimumSubarrayLength(vector<int>& nums, int k) {
        int res = INT_MAX;
        int i = 0;
        for (int j = 0; j < nums.size(); j++) {
            addOR(nums[j]);

            while (extractOR() >= k && i <= j) {
                res = min(res, j - i + 1);

                removeOR(nums[i]);
                i++;
            }
        }

        return res == INT_MAX ? -1 : res;
    }
};
```