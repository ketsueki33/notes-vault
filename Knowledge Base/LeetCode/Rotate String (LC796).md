---
difficulty: easy
leetcode-num: 796
topics:
  - String
  - String Matching
---
[Problem Link](https://leetcode.com/problems/rotate-string/)

#### Problem
Given two strings `s` and `goal`, return `true` _if and only if_ `s` _can become_ `goal` _after some number of **shifts** on_ `s`.

A **shift** on `s` consists of moving the leftmost character of `s` to the rightmost position.

- For example, if `s = "abcde"`, then it will be `"bcdea"` after one shift.

**Example 1:**

**Input:** s = "abcde", goal = "cdeab"
**Output:** true

**Example 2:**

**Input:** s = "abcde", goal = "abced"
**Output:** false

**Constraints:**

- `1 <= s.length, goal.length <= 100`
- `s` and `goal` consist of lowercase English letters.
#### Solution
##### Optimal Approach
- **Check Lengths**:
    
    - The first `if` statement checks if `s` and `goal` have the same length.
    - If `s.size()` is not equal to `goal.size()`, it’s impossible for `s` to be rotated into `goal`, so we immediately return `false`.
- **Concatenate `s` with Itself**:
    
    - The core idea here is that any rotation of `s` will appear as a substring within the string `s + s`.
    - For example, if `s = "abcde"`, then `s + s` will be `"abcdeabcde"`. Within this concatenated string, any rotation of `"abcde"` (like `"cdeab"`, `"deabc"`, etc.) will appear as a substring.
    - By doubling `s` like this, we cover all possible rotations of `s` in a single string, allowing us to simply search for `goal` as a substring.
- **Check for Substring**:
    
    - We use `(s + s).find(goal)` to check if `goal` is a substring of `s + s`.
    - `find` returns the index of the first occurrence if `goal` is found; otherwise, it returns `string::npos` (a constant indicating "not found").
    - If `find(goal) != string::npos`, this means `goal` is indeed a rotation of `s`, so we return `true`.

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    bool rotateString(string s, string goal) {
        if (s.size() != goal.size())
            return false;

        return (s + s).find(goal) != string::npos;
    }
};
```

##### Brute Force Approach

*TC ->* O( n$^{2}$ )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    bool rotateString(string s, string goal) {
        for (int i = 0; i < s.size(); i++) {
            if (s == goal)
                return true;
            s.push_back(s.front());
            s.erase(s.begin());
        }

        return false;
    }
};
```