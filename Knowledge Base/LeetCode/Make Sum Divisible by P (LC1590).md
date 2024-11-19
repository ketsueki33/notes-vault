---
difficulty: medium
leetcode-num: 1590
topics:
  - Array
  - Hash Table
  - Prefix Sum
---
[Problem Link](https://leetcode.com/problems/make-sum-divisible-by-p/)

#### Problem
Given an array of positive integers `nums`, remove the **smallest** subarray (possibly **empty**) such that the **sum** of the remaining elements is divisible by `p`. It is **not** allowed to remove the whole array.

Return _the length of the smallest subarray that you need to remove, or_ `-1` _if it's impossible_.

A **subarray** is defined as a contiguous block of elements in the array.

**Example 1:**

**Input:** `nums = [3,1,4,2], p = 6`
**Output:** 1
**Explanation:** The sum of the elements in nums is 10, which is not divisible by 6. We can remove the subarray `[4]`, and the sum of the remaining elements is 6, which is divisible by 6.

**Example 2:**

**Input:** nums = `[6,3,5,2], p = 9`
**Output:** 2
**Explanation:** We cannot remove a single element to get a sum divisible by 9. The best way is to remove the subarray `[5,2]`, leaving us with `[6,3]` with sum 9.

**Example 3:**

**Input:** `nums = [1,2,3], p = 3`
**Output:** 0
**Explanation:** Here the sum is 6. which is already divisible by 3. Thus we do not need to remove anything.

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 109`
- `1 <= p <= 109`

#### Solution

##### Optimal Approach

[Video Explanation](https://youtu.be/5jpCEfRI1sM)

- **Calculate the total remainder:**
    
    - We calculate the total sum of the array modulo `p` (`totalRem`), which gives us the remainder when the sum is divided by `p`.
    - If the total remainder is `0`, the array is already divisible by `p`, so no subarray needs to be removed, and we return `0`.
- **Prefix Sum & Hash Map Setup:**
    
    - We use a **prefix sum** approach to keep track of cumulative sums of the array. The idea is to figure out which part of the array (a subarray) can be removed to make the total sum divisible by `p`.
    - We use a **hash map** (`mp`) to store the prefix sum remainders and their corresponding indices. This helps in finding subarrays efficiently.
- **Iterating through the array:**
    
    - We calculate the **prefix sum** of the array modulo `p` as we iterate through each element.
    - The **key idea** is that the difference between the prefix sum at two different indices gives us the sum of the subarray in between those indices.
    - We want to find a subarray whose sum, when removed, will make the total sum divisible by `p`. So, we calculate the target remainder (`key`) as: 
	    $$
	    key=(prefixSum−totalRem+p)%p\text{key} = (\text{prefixSum} - \text{totalRem} + p) \% pkey=(prefixSum−totalRem+p)%p
	    $$
		This modulo operation avoids issues with negative values by ensuring that all values remain within the range `[0, p-1]`.
- **Check for valid subarray:**
    
    - If we have previously seen this **target remainder** (`key`) in the hash map, it means we can remove the subarray between the current index and the index stored in the hash map.
    - The difference between these indices gives us the length of the subarray, and we update `res` with the minimum length found so far.
- **Update the Hash Map:**
    
    - After each iteration, we update the hash map with the current **prefix sum** and its index to ensure we always have the shortest subarray to remove.
    - This ensures that future subarrays we check will be as short as possible.
- **Return the Result:**
    
    - After finishing the loop, if `res` is still equal to the size of the array, it means no valid subarray was found that can make the sum divisible by `p`. In this case, we return `-1`.
    - Otherwise, we return `res`, which is the length of the smallest subarray found.

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    int minSubarray(vector<int>& nums, int p) {
        int totalRem = 0;

        for (int x : nums)
            totalRem = (totalRem + x) % p;

        if (totalRem == 0)
            return 0;

        unordered_map<int, int> mp;
        int prefixSum = 0;
        int res = nums.size(); // Start with max possible length (nums.size())

        mp[0] = -1; // base case: prefix sum is 0 before array starts

        for (int i = 0; i < nums.size(); i++) {
            prefixSum = (prefixSum + nums[i]) % p; // Update prefix sum modulo p

            // Target remainder: (prefixSum - totalSum) % p. Handle negative
            // modulo by adding p.
            int key = (prefixSum - totalRem + p) % p;

            // If we have seen this remainder before, it means we can remove a
            // subarray
            if (mp.count(key))
                res = min(res, i - mp[key]);

            // Update map with the current prefix sum and its index to ensure we
            // are finding shortest subaarray
            mp[prefixSum] = i;
        }

        if (res == nums.size())
            return -1;

        return res;
    }
};
```