---
difficulty: medium
leetcode-num: 1593
topics:
  - String
  - Backtracking
  - Hash Table
---
[Problem Link](https://leetcode.com/problems/split-a-string-into-the-max-number-of-unique-substrings/)

#### Problem
Given a string `s`, return _the maximum number of unique substrings that the given string can be split into_.

You can split string `s` into any list of **non-empty substrings**, where the concatenation of the substrings forms the original string. However, you must split the substrings such that all of them are **unique**.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "ababccc"
**Output:** 5
**Explanation**: One way to split maximally is ['a', 'b', 'ab', 'c', 'cc']. Splitting like ['a', 'b', 'a', 'b', 'c', 'cc'] is not valid as you have 'a' and 'b' multiple times.

**Example 2:**

**Input:** s = "aba"
**Output:** 2
**Explanation**: One way to split maximally is ['a', 'ba'].

**Example 3:**

**Input:** s = "aa"
**Output:** 1
**Explanation**: It is impossible to split the string any further.

**Constraints:**

- `1 <= s.length <= 16`
    
- `s` contains only lower case English letters.

#### Solution
##### Optimal Approach
1. **Backtracking Setup:**
- The function `maxUniqueSplit` calls a helper backtracking function that explores different ways to split the string into substrings. The main goal is to maximize the number of unique substrings, which are tracked using a set (`st`).

2. **Base Case:**

- When the `idx` (current index in the string) reaches the size of the string (`idx == s.size()`), it means we have traversed the entire string. At this point, no further splits can be made, so we return `0` to indicate that no more substrings can be formed in this path.

3. **Backtracking Logic:**

- The function keeps building a substring `curr` by adding characters from the string as it moves through the indices (`curr += s[idx]`).
- **Two Choices:**
    - **Include the current substring (push it)**: If the `curr` substring has not been used before (checked via `st.count(curr)`), the substring is added to the set (`st.insert(curr)`), and the function makes a recursive call with an empty `curr` to start forming the next substring. This adds `1` to the count of unique substrings.
    - **Skip the current substring (not push it)**: The function continues to add more characters to the `curr` substring without adding it to the set, making a recursive call to try splitting later (`backtrack(s, idx + 1, curr)`).

4. **Tracking the Maximum Count:**

- For each step, the solution computes two values:
    - `push`: The count of unique substrings if the current substring `curr` is pushed into the set (i.e., treated as a unique substring).
    - `notPush`: The count if we decide not to push the current substring at this point and keep adding characters to `curr`.
- It returns the maximum of the two (`max(push, notPush)`) to explore both possibilities and ensure the maximum number of unique substrings.

5. **Backtracking (Undo the Decision):**

- After making the recursive call with the `push` decision, the substring is removed from the set (`st.erase(curr)`) to backtrack and explore other possibilities in the recursion tree.


*TC ->* O( 2$^{n}$ )
*SC ->* O( n )

```cpp title=Code
class Solution {
    unordered_set<string> st;

    int backtrack(string& s, int idx, string curr) {
        if (idx == s.size())
            return 0;

        curr += s[idx];
        int push = -1;

        if (!st.count(curr)) {
            st.insert(curr);
            push = 1 + backtrack(s, idx + 1, "");

            st.erase(curr); // backtrack
        }

        int notPush = backtrack(s, idx + 1, curr);

        return max(push, notPush);
    }

public:
    int maxUniqueSplit(string s) { return backtrack(s, 0, ""); }
};
```