---
difficulty: medium
leetcode-num: 2563
topics:
  - Array
  - Two Pointer
  - Binary Search
  - Sorting
---
[Problem Link](https://leetcode.com/problems/count-the-number-of-fair-pairs/)

#### Problem
Given a **0-indexed** integer array `nums` of size `n` and two integers `lower` and `upper`, return _the number of fair pairs_.

A pair `(i, j)` is **fair** if:

- `0 <= i < j < n`, and
- `lower <= nums[i] + nums[j] <= upper`

**Example 1:**

**Input:** nums = [0,1,7,4,4,5], lower = 3, upper = 6
**Output:** 6
**Explanation:** There are 6 fair pairs: (0,3), (0,4), (0,5), (1,3), (1,4), and (1,5).

**Example 2:**

**Input:** nums = [1,7,9,2,5], lower = 11, upper = 11
**Output:** 1
**Explanation:** There is a single fair pair: (2,3).

**Constraints:**

- `1 <= nums.length <= 105`
- `nums.length == n`
- `-109 <= nums[i] <= 109`
- `-109 <= lower <= upper <= 109`

#### Solution
[Video Explanation](https://youtu.be/r3EnymXRC9A)

##### Optimal Approach
1. **Sort the Array**  
    The array `nums` is sorted in ascending order. Sorting allows us to use binary search (`lower_bound` and `upper_bound`) to efficiently find pairs of values that meet the range criteria.
    
2. **Loop Through Elements**  
    We iterate through each element `nums[i]` from `i = 0` to `i = nums.size() - 1`.
    
3. **Using Binary Search for Range of Pairs**  
    For each `nums[i]`, we look for values `nums[j]` (where `j > i`) such that `nums[i] + nums[j]` is within `[lower, upper]`:
    
    - **Finding the Lower Bound**
        
        - We use `lower_bound` to find the first position `idx` where the value at `nums[idx]` is greater than or equal to `lower - nums[i]`. This position represents the smallest value for `nums[j]` that could form a sum of at least `lower` with `nums[i]`.
        - Subtracting `i` from `idx - 1` gives us `x`, the count of elements before this index that are too low to meet the lower bound.
    - **Finding the Upper Bound**
        
        - We use `upper_bound` to find the position `idx` where the value `nums[idx]` is greater than `upper - nums[i]`. This position represents the smallest value beyond the upper bound for pairs with `nums[i]`.
        - Subtracting `i` from `idx - 1` gives us `y`, the count of elements up to this point that still meet the upper limit.
4. **Counting Fair Pairs**
    
    - The number of valid pairs with `nums[i]` is calculated by subtracting the lower bound index `x` from the upper bound index `y`. This gives the number of valid values `nums[j]` where `i < j` and `nums[i] + nums[j]` is within `[lower, upper]`.
    - Add this count to `res`.
5. **Return Result**  
    Finally, `res` is returned as the total count of fair pairs.



*TC ->* O( `n log n` )
*SC ->* O( 1 )

```cpp title=Code
class Solution {

public:
    long long countFairPairs(vector<int>& nums, int lower, int upper) {
        long long res = 0;

        sort(nums.begin(), nums.end());

        for (int i = 0; i < nums.size(); i++) {

            int idx =
                lower_bound(nums.begin() + i + 1, nums.end(), lower - nums[i]) -
                nums.begin();
            int x = idx - 1 - i;

            idx =
                upper_bound(nums.begin() + i + 1, nums.end(), upper - nums[i]) -
                nums.begin();
            int y = idx - 1 - i;

            res += (y - x);
        }

        return res;
    }
};
```