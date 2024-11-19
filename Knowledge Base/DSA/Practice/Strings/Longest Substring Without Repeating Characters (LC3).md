---
difficulty: medium
leetcode-num: 3
topics:
  - String
  - Sliding Window
  - Hash Table
---
[Problem Link](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

#### Problem
Given a string `s`, find the length of the **longest**

**substring**

without repeating characters.

**Example 1:**

**Input:** s = `"abcabcbb"`
**Output:** 3
**Explanation:** The answer is `"abc"`, with the length of 3.

**Example 2:**

**Input:** s = `"bbbbb"`
**Output:** 1
**Explanation:** The answer is `"b"`, with the length of 1.

**Example 3:**

**Input:** s = `"pwwkew"`
**Output:** 3
**Explanation:** The answer is `"wke"`, with the length of 3.
Notice that the answer must be a substring, `"pwke"` is a subsequence and not a substring.

**Constraints:**

- `0 <= s.length <= 5 * 104`
- `s` consists of English letters, digits, symbols and spaces.

#### Solution
[Video Explanation](https://www.youtube.com/watch?v=qtVh-XEpsJo)

##### Optimal Approach

We will be using a hash map to keep track of a character's last appearance. A sliding window will keep expanding till a repeating character is found in which case we will move the start of the sliding window to index just after the repeating character's last appearance because we know that all characters after that in the sliding window are still unique.

We first initialize a hash table and two variables:
- `unordered_map<char, int> mp`: Stores characters and their last seen position.
- `curr_start`: Start index of the current substring being considered.
- `res`: Length of the longest substring found so far.

**Inside the main  loop:** 

For each character `ch` in the string:
- If `ch` is new or its last position is before `curr_start`:
    - Update `res` if current substring is longer.
- Else (repeating character found):
    - Move `curr_start` to just after the last occurrence of `ch`.
- Update `ch`'s position in the map.


*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> mp;

        int curr_start = 0;
        int res = 0;

        for (int i = 0; i < s.size(); i++) {
            char ch = s[i];
            if (mp[ch] < curr_start || mp.count(ch) == 0) {
                res = max(res, i - curr_start + 1);
            } else {
                curr_start = mp[ch] + 1;
            }
            mp[ch] = i;
        }

        return res;
    }
};
```

##### Brute Force
This approach consists of taking the two loops one for traversing the string and another loop - nested loop for finding different substrings and after that, we will check for all substrings one by one and check for each and every element if the element is not found then we will store that element in HashSet otherwise we will break from the loop and count it.

*TC ->* O( n$^{2}$ )
*SC ->* O( n )