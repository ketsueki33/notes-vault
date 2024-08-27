---
difficulty: medium
leetcode-num: 56
topics:
  - Array
  - Sorting
---
[Problem Link](https://leetcode.com/problems/merge-intervals/)

#### Problem
Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return _an array of the non-overlapping intervals that cover all the intervals in the input_.

**Example 1:**

**Input:** intervals =` [[1,3],[2,6],[8,10],[15,18]]`
**Output:** `[[1,6],[8,10],[15,18]]`
**Explanation:** Since intervals `[1,3]` and `[2,6]` overlap, merge them into `[1,6]`.

**Example 2:**

**Input:** intervals = `[[1,4],[4,5]]`
**Output:** `[[1,5]]`
**Explanation:** Intervals`[1,4]` and `[4,5]` are considered overlapping.

**Constraints:**

- `1 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 104`

#### Solution
[Video Explanation](https://youtu.be/IexN60k62jo)

##### Optimal Approach (easier)
We can solve this in a single iteration after sorting.

1. Sort the intervals:
    - Sort the input intervals based on their start times.
2. Initialize result vector:
    - Create an empty vector `res` to store merged intervals.
3. Iterate through sorted intervals:
    - For each interval `x`:
    - a. If `res` is empty OR `x` doesn't overlap with the last interval in `res`:
        - Add `x` to `res` as a new interval.
    - b. If `x` overlaps with the last interval in `res`:
        - Merge `x` with the last interval in `res` by updating the end time.
4. Return the result:
    - `res` now contains all merged intervals.

*Key points to remember:*
- Sorting allows easy comparison of adjacent intervals.
- We only need to compare with the last interval in `res`.
- Overlapping occurs when the start of the current interval is <= the end of the last merged interval.
- When merging, we take the maximum of the current end and the new interval's end.

*TC ->* O( `n log n + n` )
*SC ->* O( `n` ) if counting result vector.

```cpp title=Code
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        int n = intervals.size();
        sort(intervals.begin(), intervals.end());

        vector<vector<int>> res;

        for (auto x : intervals) {
            if (res.empty() || x.front() > res.back().back())
                res.push_back(x);
            else {
                res.back()[1] = max(res.back()[1], x.back());
            }
        }
        return res;
    }
};
```

##### Brute Force Approach

*TC ->* O( `n log n + 2n` )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        int n = intervals.size();
        sort(intervals.begin(), intervals.end());

        vector<vector<int>> res;

        for (int i = 0; i < n; i++) {
            int start = intervals[i][0];
            int end = intervals[i][1];

            if (!res.empty() && end <= res.back()[1])
                continue;

            for (int j = i + 1; j < n; j++) {
                if (intervals[j][0] <= end)
                    end = max(end, intervals[j][1]);
                else
                    break;
            }
            res.push_back({start, end});
        }
        return res;
    }
};
```