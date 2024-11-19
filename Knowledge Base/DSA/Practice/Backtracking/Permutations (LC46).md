---
difficulty: medium
leetcode-num: 46
topics:
  - Backtracking
  - Array
---
[Problem Link](https://leetcode.com/problems/permutations/)

#### Problem
Given an array `nums` of distinct integers, return _all the possible permutations_. You can return the answer in **any order**.

**Example 1:**

**Input:** nums = `[1,2,3]`
**Output:** `[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]`

**Example 2:**

**Input:** nums = `[0,1]`
**Output:**`[[0,1],[1,0]]`

**Example 3:**

**Input:** nums = `[1]`
**Output:** `[[1]]`

**Constraints:**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- All the integers of `nums` are **unique**.

#### Solution
[Video Explanation](https://youtu.be/s7AvT7cGdSo)
##### Optimal Approach
The following diagram explains our approach. We are swapping each character and then backtracking. This way we can get all permutations.

![[Permutations.png]]


> [!Warning] This algorithm will generate duplicate strings if there are repeating characters.


*TC ->* O( n * n!  ) 
For each permutation, the algorithm does a constant amount of work (O(1) per swap) and there are  n swaps for each permutation.

*SC ->* O( n * n! ) - To store `n!` permutations with each having length `n`. 

```cpp title=Code
class Solution {
public:

    void backtrack(vector<int> &nums, vector<vector<int>> &res, int index = 0){
        if( index == nums.size()){
            res.push_back(nums);
            return;
        }

        for( int i = index ; i < nums.size() ; i++ ){
            swap(nums[index],nums[i]);
            backtrack(nums,res,index+1);
            swap(nums[index],nums[i]);
        }

    }

    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> res;
        backtrack(nums,res);
        return res;

    }
};
```