---
difficulty: medium
leetcode-num: 2044
topics:
  - Array
  - Backtracking
  - Bit Manipulation
  - Enumeration
---
[Problem Link](https://leetcode.com/problems/count-number-of-maximum-bitwise-or-subsets/)

#### Problem
Given an integer array `nums`, find the **maximum** possible **bitwise OR** of a subset of `nums` and return _the **number of different non-empty subsets** with the maximum bitwise OR_.

An array `a` is a **subset** of an array `b` if `a` can be obtained from `b` by deleting some (possibly zero) elements of `b`. Two subsets are considered **different** if the indices of the elements chosen are different.

The bitwise OR of an array `a` is equal to `a[0] **OR** a[1] **OR** ... **OR** a[a.length - 1]` (**0-indexed**).

**Example 1:**

**Input:** nums = [3,1]
**Output:** 2
**Explanation:** The maximum possible bitwise OR of a subset is 3. There are 2 subsets with a bitwise OR of 3:
- [3]
- [3,1]

**Example 2:**

**Input:** nums = [2,2,2]
**Output:** 7
**Explanation:** All non-empty subsets of [2,2,2] have a bitwise OR of 2. There are 23 - 1 = 7 total subsets.

**Example 3:**

**Input:** nums = [3,2,1,5]
**Output:** 6
**Explanation:** The maximum possible bitwise OR of a subset is 7. There are 6 subsets with a bitwise OR of 7:
- [3,5]
- [3,1,5]
- [3,2,5]
- [3,2,1,5]
- [2,5]
- [2,1,5]

**Constraints:**

- `1 <= nums.length <= 16`
- `1 <= nums[i] <= 105`
#### Solution

##### Optimal Approach (DP)

1. **Goal**:
    
    - We aim to count all subsets of the array `nums` whose bitwise OR equals the maximum possible OR value that can be obtained from any subset.
2. **maxOR Calculation**:
    
    - Before the dynamic programming (DP) part begins, we calculate the maximum possible OR value (`maxOR`) by OR-ing all elements in the array. This gives the highest possible OR value that can be achieved by any subset.
3. **DP Array (`dp`)**:
    
    - We initialize a DP array `dp` of size `maxOR + 2`. Each element `dp[i]` will store the number of ways to form subsets that produce an OR value of `i`.
    - Initially, `dp[0] = 1` because there's exactly one way to have an OR value of 0: by choosing the empty subset.
4. **DP Transitions**:
    
    - For each element `x` in `nums`, we process the DP array in **reverse order** (starting from `maxOR` down to 0). This ensures that when updating `dp[i|x]`, we are not overwriting values that we still need to process for this round.
    - The expression `dp[i|x] += dp[i]` means:
        - We take the number of ways to form subsets with OR value `i` (from previous rounds) and update the count for the OR value `i|x`, which is the OR of `i` and the current number `x`.
        - This accounts for the subsets that include the current element `x` and modifies the OR value accordingly.
5. **Return the Result**:
    
    - After processing all elements, `dp[maxOR]` will contain the number of subsets whose OR value equals the maximum possible OR value (`maxOR`).

###### Key Points:

- **Reverse DP Update**: We update the DP array in reverse to ensure that the OR combinations for the current round are based on the values from the previous round and don't overwrite themselves during processing.
- **Efficiency**: Instead of explicitly generating all subsets (as in backtracking), this approach counts the subsets indirectly using bitwise operations and a DP array, which is more efficient.
- **Why `maxOR + 2`**: This is a safe size for the DP array to ensure that the largest OR value and any possible increments during the process fit within the array size.
*TC ->* O( `n * maxOR` )
*SC ->* O( maxOR )

```cpp title=Code
class Solution {
public:
    int countMaxOrSubsets(vector<int>& nums) {
        int maxOR = 0;

        for (int x : nums)
            maxOR |= x;

        vector<int> dp(maxOR + 2, 0);
        dp[0] = 1;

        for (int x : nums) {
            for (int i = maxOR; i >= 0; i--)
                dp[i | x] += dp[i];
        }

        return dp[maxOR];
    }
};
```

##### Alright Approach (Backtracking)
- **Goal**:
    
    - We want to count all subsets whose bitwise OR equals the maximum OR value that can be obtained using any subset of the array.
- **maxOR Calculation**:
    
    - Before the backtracking starts, the maximum possible OR value (`maxOR`) is calculated by taking the OR of all elements in the array. This is because OR-ing all elements gives the highest possible value we can get.
- **Backtracking**:
    
    - The backtracking function explores all possible subsets by deciding for each element whether to **include** it in the current subset or **exclude** it.
    - Two recursive calls are made for every element:
        - **Include the element**: The current OR value is updated by OR-ing the current number.
        - **Exclude the element**: The current OR value remains unchanged.
    - The base case occurs when we have considered all elements. If the OR of the current subset equals the `maxOR`, we increment the result count (`res`).
- **Parameters**:
    
    - `nums`: The input array.
    - `maxOR`: The precomputed maximum OR value we want to match.
    - `res`: This keeps track of how many subsets achieve the `maxOR`.
    - `idx`: The current index being processed in the backtracking.
    - `currOR`: The OR value of the current subset being formed.

**Key Points**:
- **Base Case**: If we reach the end of the array (i.e., `idx == nums.size()`), we check if the current OR (`currOR`) matches the `maxOR`. If so, we increment the result counter.
- **Recursive Exploration**: For each element, we either include it in the current subset or exclude it, exploring all possible subsets.

*TC ->* O( 2$^{n}$ )
*SC ->* O( n ), due to recursion depth

```cpp title=Code
class Solution {
    void backtrack(vector<int>& nums, int maxOR, int& res, int idx = 0,
                   int currOR = 0) {
        if (idx == nums.size()) {
            if (currOR == maxOR)
                res++;
            return;
        }

        int num = nums[idx];
        // include
        backtrack(nums, maxOR, res, idx + 1, (currOR | num));

        // exclude
        backtrack(nums, maxOR, res, idx + 1, currOR);
    }

public:
    int countMaxOrSubsets(vector<int>& nums) {
        int maxOR = 0;
        int res = 0;

        for (int x : nums)
            maxOR |= x;

        backtrack(nums, maxOR, res);

        return res;
    }
};
```