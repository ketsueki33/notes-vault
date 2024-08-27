---
difficulty: medium
leetcode-num: 39
topics:
  - Backtracking
  - Array
---
[Problem Link](https://leetcode.com/problems/combination-sum/)

#### Problem
Given an array of **distinct** integers `candidates` and a target integer `target`, return _a list of all **unique combinations** of_ `candidates` _where the chosen numbers sum to_ `target`_._ You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the

frequency

of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

**Example 1:**

**Input:** candidates = `[2,3,6,7]`, target = 7
**Output:** `[[2,2,3],[7]]`
**Explanation:**
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.

**Example 2:**

**Input:** candidates = `[2,3,5]`, target = 8
**Output:** `[[2,2,2,2],[2,3,3],[3,5]]`

**Example 3:**

**Input:** candidates = `[2]`, target = 1
**Output:** []

**Constraints:**

- `1 <= candidates.length <= 30`
- `2 <= candidates[i] <= 40`
- All elements of `candidates` are **distinct**.
- `1 <= target <= 40`

#### Solution
[Video Explanation](https://youtu.be/OyZFFqQtu98?list=PLgUwDviBIf0p4ozDR_kJJkONnb1wdx2Ma)

##### Optimal Approach
 At every index we will have two choices:
  
1. *pick the number at that index*. Since we can reuse numbers, we will call our recursive function for the same index even after picking.
2. *don't pick the number at that index*. we can go to the next index in the array.

The base case is when we have reached the target or if we have reached the end of the array.


> [!warning]  Note:
> Remember to put the  *picking  current number* step inside an **if** condition that checks whether we have crossed the target or not. Otherwise we will get an infinite loop ( where the first step is being executed endlessly )


*TC ->* O( 2$^{n}$ )
*SC ->* O( n  ) (after ignoring auxiliary space for storing result)

```cpp title=Code
class Solution {
    vector<vector<int>> res;

public:
    void backtrack(vector<int>& arr, int target, int ind, vector<int>& v) {
        if (ind == arr.size()) {
            if (target == 0)
                res.push_back(v);
            return;
        }
        // pick current index
        if (arr[ind] <= target) {
            v.push_back(arr[ind]);
            backtrack(arr, target - arr[ind], ind, v);
            v.pop_back();
        }

        // move to next index without picking
        backtrack(arr, target, ind + 1, v);
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int> v;
        backtrack(candidates, target, 0, v);
        return res;
    }
};
``` 

#### Similar Problems
- [[Combination Sum II (LC40)]]