---
difficulty: medium
leetcode-num: 2707
topics:
  - String
  - Hash Table
  - Dynamic Programming
---
[Problem Link](https://leetcode.com/problems/extra-characters-in-a-string/)

#### Problem
You are given a **0-indexed** string `s` and a dictionary of words `dictionary`. You have to break `s` into one or more **non-overlapping** substrings such that each substring is present in `dictionary`. There may be some **extra characters** in `s` which are not present in any of the substrings.

Return _the **minimum** number of extra characters left over if you break up_ `s` _optimally._

**Example 1:**

**Input:** s = `"leetscode"`, dictionary = `["leet","code","leetcode"]`
**Output:** 1
**Explanation:** We can break s in two substrings: "leet" from index 0 to 3 and "code" from index 5 to 8. There is only 1 unused character (at index 4), so we return 1.

**Example 2:**

**Input:** s = `"sayhelloworld"`, dictionary = `["hello","world"]`
**Output:** 3
**Explanation:** We can break s in two substrings: "hello" from index 3 to 7 and "world" from index 8 to 12. The characters at indices 0, 1, 2 are not used in any substring and thus are considered as extra characters. Hence, we return 3.

**Constraints:**

- `1 <= s.length <= 50`
- `1 <= dictionary.length <= 50`
- `1 <= dictionary[i].length <= 50`
- `dictionary[i]` and `s` consists of only lowercase English letters
- `dictionary` contains distinct words

#### Solution
[Video Explanation](https://youtu.be/8l76FiMqYHg)

##### DP Tabulation Approach

- The algorithm works **backwards** from the end of the string `s` to the start (`i = n - 1` down to `0`).
- For each starting index `i`, the default assumption is that the current character `s[i]` is "extra," so the value `dp[i]` is initialized as `1 + dp[i + 1]`, which adds 1 extra character and moves to the next index.
- The algorithm then tries to match every substring starting from `i` to `j` with words in the dictionary. If a substring `s[i:j]` is found in the dictionary (`hash.count(curr) == 1`), it updates `dp[i]` by minimizing the result with `dp[j + 1]`, meaning no extra characters are counted for this valid word.

_TC ->_ O( n )  
_SC ->_ O( words )

```cpp title=Code
class Solution {

public:
    int minExtraChar(string s, vector<string>& dict) {
        int n = s.size();
        vector<int> dp(n + 1, 0);
        unordered_set<string> hash(dict.begin(), dict.end());

        for (int i = n - 1; i >= 0; i--) {
            dp[i] = 1 + dp[i + 1];

            for (int j = i; j < n; j++) {
                string curr = s.substr(i, j - i + 1);

                if (hash.count(curr) == 1)
                    dp[i] = min(dp[i], dp[j + 1]);
            }
        }

        return dp[0];
    }
};
```

##### DP Recursive Approach
###### Key Parts of the Solution:

1. **Dynamic Programming Array (`dp`)**:
    
    - The `dp` array is used to store the results of subproblems, i.e., the minimum number of extra characters for each starting position in the string `s`. It allows us to avoid recomputation, thereby optimizing the solution.
2. **Hash Set (`hash`)**:
    
    - This is an unordered set that stores all the words from the dictionary for fast lookup. By storing the words in a hash set, we can efficiently check if any substring of `s` exists in the dictionary.
3. **Recursive Function (`solve`)**:
    
    - The function `solve(s, i)` recursively finds the minimum extra characters starting from index `i` in the string `s`.
    - **Base Case**: If `i` reaches the end of the string (i.e., `i == s.size()`), it returns `0`, meaning no extra characters remain.
    - **Memoization**: If `dp[i]` is not `-1`, it means the result for this subproblem has already been computed, and it directly returns `dp[i]`.
    - **Recursive Exploration**: The function first assumes the character at `i` is an "extra" character and calls `solve(i + 1)` to check the result from the next position.
    - **Dictionary Match**: The function checks every possible substring from the current index `i` to the end of the string. If a substring matches a word in the dictionary, the function updates the result by minimizing the number of extra characters.
4. **Main Function (`minExtraChar`)**:
    
    - Initializes the `dp` array with `-1` to signify uncalculated subproblems.
    - Inserts all words from the dictionary into the `hash` set.
    - Finally, it calls the `solve` function starting from index `0` to compute the minimum number of extra characters for the entire string.

###### How It Works:

- For each starting position in the string `s`, the function either:
    1. Counts the current character as "extra" and proceeds to the next position.
    2. Tries to match a substring starting at that position with a word in the dictionary and, if matched, skips the matched substring and proceeds.
- It keeps track of the minimum number of extra characters by exploring all possible ways to break down the string into valid dictionary words.

*TC ->* O( n$^{3}$ )
*SC ->* O( words )

```cpp title=Code
class Solution {
    vector<int> dp;
    unordered_set<string> hash;

    int solve(string& s, int i) {
        if (i == s.size())
            return 0;

        if (dp[i] != -1)
            return dp[i];

        int result = 1 + solve(s, i + 1);

        for (int j = i; j < s.size(); j++) {
            string curr = s.substr(i, j - i + 1);
            if (hash.count(curr) == 1) {
                result = min(result, solve(s, j + 1));
            }
        }

        return dp[i] = result;
    }

public:
    int minExtraChar(string s, vector<string>& dictionary) {
        dp.resize(s.size(), -1);

        for (string& x : dictionary)
            hash.insert(x);

        return solve(s, 0);
    }
};

```