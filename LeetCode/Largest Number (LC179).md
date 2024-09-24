---
difficulty: medium
leetcode-num: 179
topics:
  - Array
  - String
  - Greedy
  - Sorting
---
[Problem Link](https://leetcode.com/problems/largest-number/)

#### Problem
Given a list of non-negative integers `nums`, arrange them such that they form the largest number and return it.

Since the result may be very large, so you need to return a string instead of an integer.

**Example 1:**

**Input:** nums = `[10,2]`
**Output:** "210"

**Example 2:**

**Input:** nums = `[3,30,34,5,9]`
**Output:** "9534330"

**Constraints:**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 109`
#### Solution
**1. Convert Integers to Strings**:
- Since concatenating numbers depends on their string representation, we first convert all integers in the input `nums` array into strings.
- For example, the number `10` becomes `"10"`, and `2` becomes `"2"`. These string versions of the numbers are stored in the vector `v`.

**2. Custom Comparator for Sorting**:
- The `myCmp` function is used as a custom comparator for sorting the strings.
- **Logic**: Given two strings `a` and `b`, we compare `a + b` with `b + a`.
- If `a + b` is greater than `b + a`, then `a` should appear before `b` in the sorted array, and vice versa.
- This ensures that when we concatenate the strings in the sorted order, we get the largest possible number.

**3. Sorting**:
- We sort the array of strings `v` using the custom comparator `myCmp`. This will arrange the strings in such a way that their concatenation forms the largest possible number.

**4. Edge Case for Leading Zeros**:
- After sorting, the largest number should be formed by concatenating the elements in `v`.
- However, there is a special case where the input array contains only zeros or leading zeros. For example, if the input is `[0, 0]`, the output should be `"0"`, not `"00"`.
- To handle this, we check if the first element in the sorted array is `"0"`. If so, we return `"0"`.

**5. Concatenation**:
- We concatenate all the strings in `v` to form the result.

*TC ->* O(  `n log n * k`  )
*SC ->* O( `n * k` )
where n is the number of elements in `nums`, k is the maximum number of digits in the numbers

```cpp title=Code
class Solution {
    static bool myCmp(string& a, string& b) { return a + b > b + a; }

public:
    string largestNumber(vector<int>& nums) {
        vector<string> v;
        string res;

        for (int x : nums) {
            v.push_back(to_string(x));
        }

        sort(v.begin(), v.end(), myCmp);

        if (v.front() == "0")
            return "0";

        for (string& x : v)
            res += x;

        return res;
    }
};
```