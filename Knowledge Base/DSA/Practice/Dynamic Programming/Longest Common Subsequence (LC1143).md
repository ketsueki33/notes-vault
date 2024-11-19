---
difficulty: medium
topics:
  - Dynamic Programming
  - String
leetcode-num: 1143
---
[Problem Link](https://leetcode.com/problems/longest-common-subsequence/)

#### Problem
Given two strings `text1` and `text2`, return _the length of their longest **common subsequence**._ If there is no **common subsequence**, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

**Example 1:**

**Input:** text1 = "abcde", text2 = "ace" 
**Output:** 3  
**Explanation:** The longest common subsequence is "ace" and its length is 3.

**Example 2:**

**Input:** text1 = "abc", text2 = "abc"
**Output:** 3
**Explanation:** The longest common subsequence is "abc" and its length is 3.

**Example 3:**

**Input:** text1 = "abc", text2 = "def"
**Output:** 0
**Explanation:** There is no such common subsequence, so the result is 0.

**Constraints:**

- `1 <= text1.length, text2.length <= 1000`
- `text1` and `text2` consist of only lowercase English characters.

#### Solution
[Video Explanation](https://youtu.be/sSno9rV8Rhg)
##### Tabulation DP Approach
We initialize a table to start our calculation. We add an extra row and column at the start filled with 0s. Each row represents a character from string 2 while each column represents a character from string 1. 

We traverse the table row wise and we check if *the corresponding characters from the two strings match*:
1. If they do: We add 1 to the top-left cell and store it in the current cell.
2. If they don't: We take max of left and top cell and store it in the current cell.

In the end, the cell `dp[n][m]` will have the length of the longest common subsequence.

![[Longest Common Subsequence (LC1143) 2024-08-12 16.39.59.excalidraw|550]]

*TC ->* O( `m * n`  )
*SC ->* O( `m * n` )

```cpp title=Code
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int n = text1.size(), m = text2.size();
        int dp[n + 1][m + 1];
        memset(dp, 0, sizeof(dp));

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (text1[i - 1] == text2[j - 1])
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                else
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }

        return dp[n][m];
    }
};
```

##### Memoization DP Approach
Same as recursive approach but we memoize the results of each call.
*TC ->* O( `m * n`  )
*SC ->* O( `m * n` )

```cpp title=Code
class Solution {
public:
    int helper(string& s1, string& s2, int i, int j, int memo[][1000]) {
        if (i == s1.size() || j == s2.size())
            return 0;

        if (memo[i][j] != -1)
            return memo[i][j];

        if (s1[i] == s2[j]) {
            memo[i][j] = 1 + helper(s1, s2, i + 1, j + 1, memo);
        }

        else
            memo[i][j] = max(helper(s1, s2, i + 1, j, memo),
                             helper(s1, s2, i, j + 1, memo));

        return memo[i][j];
    }
    int longestCommonSubsequence(string text1, string text2) {
        int n = text1.size(), m = text2.size();
        int memo[1000][1000];
        memset(memo, -1, sizeof(memo));

        return helper(text1, text2, 0, 0, memo);
    }
};
```

##### Recursive Approach ( Brute Force )

*TC ->* O( 2$^{n}$  )
*SC ->* O( n  )

```cpp title=Code
class Solution {
public:
    int helper(string s1, string s2, int i, int j) {
        if (i == s1.size() || j == s2.size())
            return 0;
        if (s1[i] == s2[j])
            return 1 + helper(s1, s2, i + 1, j + 1);

        return max(helper(s1, s2, i + 1, j), helper(s1, s2, i, j + 1));
    }
    int longestCommonSubsequence(string text1, string text2) {
        return helper(text1,text2,0,0);
    }
};
```

