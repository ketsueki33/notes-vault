---
difficulty: medium
leetcode-num: 3163
topics:
  - String
---
[Problem Link](https://leetcode.com/problems/string-compression-iii/)

#### Problem
Given a string `word`, compress it using the following algorithm:

- Begin with an empty string `comp`. While `word` is **not** empty, use the following operation:
    - Remove a maximum length prefix of `word` made of a _single character_ `c` repeating **at most** 9 times.
    - Append the length of the prefix followed by `c` to `comp`.

Return the string `comp`.

**Example 1:**

**Input:** word = "abcde"

**Output:** "1a1b1c1d1e"

**Explanation:**

Initially, `comp = ""`. Apply the operation 5 times, choosing `"a"`, `"b"`, `"c"`, `"d"`, and `"e"` as the prefix in each operation.

For each prefix, append `"1"` followed by the character to `comp`.

**Example 2:**

**Input:** word = "aaaaaaaaaaaaaabb"

**Output:** "9a5a2b"

**Explanation:**

Initially, `comp = ""`. Apply the operation 3 times, choosing `"aaaaaaaaa"`, `"aaaaa"`, and `"bb"` as the prefix in each operation.

- For prefix `"aaaaaaaaa"`, append `"9"` followed by `"a"` to `comp`.
- For prefix `"aaaaa"`, append `"5"` followed by `"a"` to `comp`.
- For prefix `"bb"`, append `"2"` followed by `"b"` to `comp`.

**Constraints:**

- `1 <= word.length <= 2 * 105`
- `word` consists only of lowercase English letters.

#### Solution

##### Optimal Approach
- **Initialize Variables**
    
    - `res`: an empty string to build the compressed result.
    - `currCh`: stores the current character we are counting (starts as the first character in `word`).
    - `currCount`: a counter for consecutive occurrences of `currCh`, starting at `0`.
- **Add a Sentinel Character**
    
    - `word.push_back('*');`: a sentinel character `'*'` is added to the end of `word`. This marks the end of the string, ensuring that the final sequence of characters gets added to `res` after the loop finishes.
- **Loop Through Each Character in `word`**
    
    - The loop iterates over each character `ch` in `word`.
        
    - **If `ch` Matches `currCh` and `currCount` is Less Than 9**:
        
        - `currCount++`: increment `currCount` by 1.
        - The check `currCount < 9` prevents the count from exceeding a single digit (a constraint sometimes used in compression algorithms for brevity).
    - **If `ch` Does Not Match `currCh` or `currCount` is 9**:
        
        - **Add `currCount` and `currCh` to `res`**:
            - `res.push_back(currCount + '0');`: convert `currCount` to a character and add it to `res`.
            - `res.push_back(currCh);`: add `currCh` to `res`.
        - **Reset Counters for the New Character**:
            - `currCh = ch;`: set `currCh` to `ch`, the new character we’re now tracking.
            - `currCount = 1;`: reset `currCount` to 1 because we’ve encountered the first occurrence of the new `currCh`.
- **Return the Compressed Result**
    
    - After the loop, `res` contains the compressed version of `word`.

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    string compressedString(string word) {
        string res;
        char currCh = word.front();
        char currCount = 0;

        word.push_back('*');

        for (char ch : word) {
            if (currCount < 9 && currCh == ch) {
                currCount++;
            } else {
                res.push_back(currCount + '0');
                res.push_back(currCh);
                currCh = ch;
                currCount = 1;
            }
        }

        return res;
    }
};
```