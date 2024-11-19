---
difficulty: medium
leetcode-num: 539
topics:
  - Array
  - String
  - Sorting
---
[Problem Link](https://leetcode.com/problems/minimum-time-difference/)

#### Problem
Given a list of 24-hour clock time points in **"HH:MM"** format, return _the minimum **minutes** difference between any two time-points in the list_.

**Example 1:**

**Input:** `timePoints = ["23:59","00:00"]`
**Output:** 1

**Example 2:**

**Input:** `timePoints = ["00:00","23:59","00:00"]`
**Output:** 0

**Constraints:**

- `2 <= timePoints.length <= 2 * 104`
- `timePoints[i]` is in the format **"HH:MM"**.

#### Solution
##### Optimal Approach
**Step 1: Converting Time to Minutes (`processTime` function)**
- The `processTime` function is used to convert each time point from "HH
    
    " format to the total number of minutes from 00:00.
- For example, for "12:34":
    - It extracts the hours (`12`) and minutes (`34`).
    - Then, it calculates the total minutes as `12 * 60 + 34 = 754`.

**Step 2: Storing Times in Minutes**
- You first loop through all the time points and use `processTime` to convert them to minutes.
- All these times in minutes are stored in the `minutes` vector.

**Step 3: Sorting the Times**
- After converting all the time points to minutes, you sort the `minutes` vector. This step ensures that the times are in ascending order, making it easy to compute the difference between consecutive times.

**Step 4: Finding the Minimum Time Difference**
- Since the times are sorted, the smallest difference between consecutive time points can be easily found by looping through the sorted `minutes` vector and computing the difference between each consecutive pair.
- However, since time is circular (i.e., from 23:59 to 00:00), the solution also needs to check the difference between the **last** time in the sorted list and the **first** time. This is done using the formula:
    - `24*60 - (minutes.back() - minutes.front())`
    - This formula calculates the circular difference between the largest and smallest time.


*TC ->* O( `n * log n` )
*SC ->* O( n )

```cpp title=Code
class Solution {
    int processTime(string s) {
        char ch;
        int hr, min;
        stringstream ss(s);
        ss >> hr >> ch >> min;

        return hr * 60 + min;
    }

public:
    int findMinDifference(vector<string>& timePoints) {
        vector<int> minutes;
        int res = INT_MAX;

        for (string s : timePoints)
            minutes.push_back(processTime(s));

        sort(minutes.begin(), minutes.end());

        res = 24 * 60 - (minutes.back() - minutes.front());

        for (int i = 1; i < minutes.size(); i++) {
            res = min(res, minutes[i] - minutes[i - 1]);
        }

        return res;
    }
};
```