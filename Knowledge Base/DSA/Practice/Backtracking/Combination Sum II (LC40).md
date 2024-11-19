---
difficulty: medium
leetcode-num: 40
topics:
  - Backtracking
  - Array
---
[Problem Link](https://leetcode.com/problems/combination-sum-ii)

#### Problem
Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

**Example 1:**

**Input:** candidates = `[10,1,2,7,6,1,5]`, target = 8
**Output:** 
`[[1,1,6],[1,2,5],[1,7],[2,6]]`

**Example 2:**

**Input:** candidates =`[2,5,2,1,2]`, target = 5
**Output:** 
`[[1,2,2],[5]]`

**Constraints:**

- `1 <= candidates.length <= 100`
- `1 <= candidates[i] <= 50`
- `1 <= target <= 30`

#### Solution
[Video Explanation](https://youtu.be/G1fRTGRxXU8?list=PLgUwDviBIf0p4ozDR_kJJkONnb1wdx2Ma)
 
##### Optimal Approach
We will first sort the input array. This way we can check for duplicates before calling the recursive function.

*Parameters of recursive function:*
- `& arr`: input array
- `target`: to keep track of remaining sum.
- `index`: to keep track of current index
- `& v`: vector to keep track of the elements picked in the current recursive call.

*Base case of Recursion:* 
When target is 0, we will push the current tracking vector into the result.
Then we will `return` as the target is already reached.

*Main working:*
Instead of picking each element, we iterate from the passed index and only pick if the current number is non-repeating in the current iteration. This prevents duplicates in our result.

We break from the iteration if the remaining target is less than the current number( since the input array is sorted.. the next numbers will also be greater than the target).

If we have picked the current number, we will backtrack and remove it before calling recursion for the not-pick case.

*TC ->* O( 2$^{n}$ )
*SC ->* O( n  ) (after ignoring auxiliary space for storing result)

```cpp title=Code
class Solution {
    vector<vector<int>> res;

public:
    void backtrack(vector<int>& arr, int target, int index, vector<int>& v) {
        if (target == 0) {
            res.push_back(v);
            return;
        }

        for (int i = index; i < arr.size(); i++) {
            // exit if current number greater than target
            if (target < arr[i])
                break;

            // only pick if the current number is the first of it's kind in this
            // iteration
            if (i > index && arr[i] == arr[i - 1])
                continue;
            
            v.push_back(arr[i]);
            backtrack(arr, target - arr[i], i + 1, v);
            v.pop_back();
        }
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        vector<int> v;
        backtrack(candidates, target, 0, v);
        return res;
    }
};
```

##### Brute Force Approach
Similar to [[Combination Sum (LC39)]] solution but we will move 1 index ahead even after picking. 

Since we have to ensure there are no duplicate arrays, we can use a ordered set to store our arrays and then convert it into a list of arrays. 

We are unnecessarily finding the same arrays more than once in this approach.

```cpp title=Code
class Solution {
    set<vector<int>> s;
public:
    void helper( int rem, vector<int>& candidates, int ind,
                vector<int>& v) {
        if (ind == candidates.size()) {
            if (rem == 0)
                s.insert(v);
            return;
        }

        // include
        if (rem >= candidates[ind]) {
            v.push_back(candidates[ind]);
            helper( rem - candidates[ind], candidates, ind + 1, v);
            v.pop_back();
        }
        // not include
        helper(rem, candidates, ind + 1, v);
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        vector<int> v;
        vector<vector<int>> res;
        helper( target, candidates, 0, v);
        for (auto x : s) {
            res.push_back(x);
        }
        return res;
    }
};
```


#### Similar Problems
- [[Combination Sum (LC39)]]