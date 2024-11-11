---
difficulty: easy
leetcode-num: 1957
topics:
  - String
---
[Problem Link](https://leetcode.com/problems/delete-characters-to-make-fancy-string/)

#### Problem

A **fancy string** is a string where no **three** **consecutive** characters are equal.

Given a string `s`, delete the **minimum** possible number of characters from `s` to make it **fancy**.

Return _the final string after the deletion_. It can be shown that the answer will always be **unique**.

**Example 1:**

**Input:** s = "leeetcode"
**Output:** "leetcode"
**Explanation:**
Remove an 'e' from the first group of 'e's to create "leetcode".
No three consecutive characters are equal, so return "leetcode".

**Example 2:**

**Input:** s = "aaabaaaa"
**Output:** "aabaa"
**Explanation:**
Remove an 'a' from the first group of 'a's to create "aabaaaa".
Remove two 'a's from the second group of 'a's to create "aabaa".
No three consecutive characters are equal, so return "aabaa".

**Example 3:**

**Input:** s = "aab"
**Output:** "aab"
**Explanation:** No three consecutive characters are equal, so return "aab".

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists only of lowercase English letters.
#### Solution

##### Optimal Approach
- **Initialize Variables**
    
    - `res`: an empty string where we'll build our "fancy" string without any more than two consecutive characters.
    - `currChar`: initialized to `'*'` (a placeholder, as it doesn't match any letter in `s`), which will store the most recent character we’re checking.
    - `currCount`: initialized to `1`, which tracks how many times `currChar` has appeared consecutively.
- **Iterate Through Each Character**
    
    - For each character `ch` in the input string `s`:
        - **Check if the Character is Repeated**
            - If `ch` matches `currChar`, increment `currCount` to reflect that it’s the same character appearing consecutively.
        - **New Character Detected**
            - If `ch` is different from `currChar`, it means we’re seeing a new character sequence.
            - Update `currChar` to `ch` and reset `currCount` to `1` since this is the first time we’ve seen this character in a row.
- **Build the Result String**
    
    - After updating `currChar` and `currCount`, check if `currCount` is less than or equal to `2`.
        - If `currCount <= 2`, append `ch` to `res`, as this means we’re allowed to keep it (either it’s the first or second occurrence).
        - If `currCount > 2`, skip `ch` to prevent more than two consecutive occurrences.
- **Return the Result**
    
    - Finally, return `res`, which now contains the original string without any character appearing more than twice consecutively.

*TC ->* O( n )
*SC ->* O( n ), if we count `res` string

```cpp title=Code
class Solution {
public:
    string makeFancyString(string s) {
        string res = "";
        char currChar = '*';
        int currCount = 1;

        for (char ch : s) {
            if (ch == currChar)
                currCount++;
            else {
                currChar = ch;
                currCount = 1;
            }

            if (currCount <= 2)
                res += ch;
        }
        return res;
    }
};
```