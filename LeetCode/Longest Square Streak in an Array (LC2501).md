---
difficulty: medium
leetcode-num: 2501
topics:
  - Array
  - Hash Table
---
[Problem Link](https://leetcode.com/problems/longest-square-streak-in-an-array/)

#### Problem
You are given an integer array `nums`. A subsequence of `nums` is called a **square streak** if:

- The length of the subsequence is at least `2`, and
- **after** sorting the subsequence, each element (except the first element) is the **square** of the previous number.

Return _the length of the **longest square streak** in_ `nums`_, or return_ `-1` _if there is no **square streak**._

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** nums = [4,3,6,16,8,2]
**Output:** 3
**Explanation:** Choose the subsequence [4,16,2]. After sorting it, it becomes [2,4,16].
- 4 = 2 * 2.
- 16 = 4 * 4.
Therefore, [4,16,2] is a square streak.
It can be shown that every subsequence of length 4 is not a square streak.

**Example 2:**

**Input:** nums = [2,3,5,6,7]
**Output:** -1
**Explanation:** There is no square streak in nums so return -1.

**Constraints:**

- `2 <= nums.length <= 105`
- `2 <= nums[i] <= 105`

#### Solution

> [!NOTE] Max Possible Answer is 5
> If we start at smallest possible number - 2.. max streak possible is 5 as 2$^{6}$ is greater than the input constraints.

##### Optimal Approach

1. **Initialize Data Structures**

	- **Set `st`**: Create an `unordered_set<double>` called `st` containing all elements from `nums`. Using a set allows quick lookups to check if a number or its square exists in the input.
	- **Result Variable `res`**: Initialize `res` to `0`. This will store the maximum length of any square streak found.

> [!NOTE] Why use `double` for `unordered_set`?
> When calculating `sqrt(x)`, the result might not be a whole number for values that aren't perfect squares. If we were using `int` in the set, it could lead to unintended results. For instance:
> 
> - If `x = 10`, `sqrt(10)` is approximately `3.162`. Converting this to an `int` would give `3`, which might exist in the set but isn't actually related to `10`.
> 
> Using `double` ensures that we store and compare exact values, avoiding mismatches due to integer rounding.



2. **Iterate Through Each Number in `nums`**

	- For each number `x` in `nums`, perform the following checks:

3. **Skip Already-Part-of-Streak Numbers**

	- **Square Root Check**: If `sqrt(x)` is found in `st`, skip this number. This check ensures that we only start counting a streak from numbers that are the "beginning" of a streak, avoiding duplicates.

4. **Count the Square Streak**

	- Initialize a `count` variable to `0`, which will count the length of the current square streak starting from `x`.
	- Set `temp` equal to `x` (to keep track of the current number in the streak).
	- **While Loop**: Continue squaring `temp` and checking if it’s in `st`.
	    - If `temp` is in `st`, increment the `count` and update `temp` to `temp * temp` to move to the next number in the square sequence.(only if temp does not exceed square root of 10$^{5}$)

5. **Update the Result**

	- After finishing the streak for `x`, update `res` to the maximum of `res` and `count`.

6. **Return the Final Result**

	- If `res` is `1`, it means no valid square streak of length greater than `1` was found, so return `-1`.
	- Otherwise, return `res`, which now holds the length of the longest square streak.

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    int longestSquareStreak(vector<int>& nums) {
        unordered_set<double> st(nums.begin(), nums.end());
        int res = 0;

        for (int x : nums) {
            if (st.count(sqrt(x)))
                continue;
            int count = 0;
            int temp = x;

            while (st.count(temp)) {
                count++;
                if (temp > sqrt(10e5))
                    break;

                temp = temp * temp;
            }

            res = max(res, count);
        }

        return res == 1 ? -1 : res;
    }
};
```