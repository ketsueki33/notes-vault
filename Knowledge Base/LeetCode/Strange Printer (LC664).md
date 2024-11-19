---
difficulty: hard
leetcode-num: 664
topics:
  - String
  - Dynamic Programming
---
[Problem Link](https://leetcode.com/problems/strange-printer/)

#### Problem
There is a strange printer with the following two special properties:

- The printer can only print a sequence of **the same character** each time.
- At each turn, the printer can print new characters starting from and ending at any place and will cover the original existing characters.

Given a string `s`, return _the minimum number of turns the printer needed to print it_.

**Example 1:**

**Input:** s = "aaabbb"
**Output:** 2
**Explanation:** Print "aaa" first and then print "bbb".

**Example 2:**

**Input:** s = "aba"
**Output:** 2
**Explanation:** Print "aaa" first and then print "b" from the second place of the string, which will cover the existing character 'a'.

**Constraints:**

- `1 <= s.length <= 100`
- `s` consists of lowercase English letters.

#### Solution
[Video Explanation](https://youtu.be/pV3arpA0TzY)

##### Optimal Approach

_TC ->_ O( n )  
_SC ->_ O( n )

```cpp title=Code
class Solution {
    int n;
    vector<vector<int>> dp;

public:
    int solvePrint(int l, int r, string& s) {
        if (l == r)
            return 1;

        if (l > r)
            return 0;

        if (dp[l][r] != -1)
            return dp[l][r];

        int i = l + 1;
        while (i <= r && s[l] == s[i])
            i++;

        if (i == r + 1)
            return 1;
        int basic = 1 + solvePrint(i, r, s);

        int greedy = INT_MAX;

        for (int j = i; j <= r; j++) {
            if (s[j] == s[l]) {
                int ans = solvePrint(i, j - 1, s) + solvePrint(j, r, s);

                greedy = min(ans, greedy);
            }
        }

        return dp[l][r] = min(basic, greedy);
    }
    int strangePrinter(string s) {
        n = s.size();
        dp.resize(n + 1, vector<int>(n + 1, -1));

        return solvePrint(0, n - 1, s);
    }
};
```