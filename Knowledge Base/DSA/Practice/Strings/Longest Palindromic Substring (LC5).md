---
difficulty: medium
leetcode-num: 5
topics:
  - Dynamic Programming
  - String
  - Two Pointer
---
[Problem Link](https://leetcode.com/problems/longest-palindromic-substring/)

#### Problem
Given a string `s`, return _the longest palindromic substring_ in `s`.

**Example 1:**

**Input:** s = "babad"
**Output:** "bab"
**Explanation:** "aba" is also a valid answer.

**Example 2:**

**Input:** s = "cbbd"
**Output:** "bb"

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consist of only digits and English letters.

#### Solution

##### Manacher's Algorithm
[Video Explanation](https://youtu.be/YVZttWzvyw8)

Pre-process the string first. We insert special characters (like '#') between each character at the start and end. This handles both odd and even-length palindromes uniformly.

**Core Algorithm**
We will maintain an array `p` where `p[i]` represents the radius of the palindrome centered at `i`.

The idea is to use the values stored in `p` to reduce the number of expansion checking required to find palindromes. For this we have two variables:
- `l`: it stores the index of the left boundary of the *rightmost* palindrome encountered so far.
- `r`: it stores the index of the right boundary of the *rightmost* palindrome encountered so far.

Iterate through the pre-processed string to populate `p` for each index. Inside the loop:

*If the the current index `i` is less than than `r`:*
It means `i` is inside an already found palindrome. So we will find the mirror index of `i` within this palindrome and store it as `mirror`. We will use `mirror` to find if a palindrome already exists at `i` (by checking `p[mirror]`.

![[Longest Palindromic Substring (LC5) 2024-07-15 15.55.36.excalidraw|600]]

Then there are two cases:
1. the palindrome at `mirror` is completely within the boundaries of the current rightmost palindrome. In this case `p[i] = p[mirror]` 
2. The palindrome at mirror exceeds the boundary of the current rightmost palindrome. In this case we can not be sure if any characters beyond `l` will form be a part of palindrome at `i`.. so we assign `p[i] = r-i` where (`r-i = mirror - l`) which includes the characters from `mirror` to `l`.

*If the current index `i`  is more than or equal to `r`:*
This means we cannot use previously found palindromes to find `p[i]` and must start with `0`. Since, we initialized the vector `p` with zeroes, we don't need to set it again as `0`.

---

We then try to expand the palindrome centered at `i`:
- We start from a radius where we know it's a palindrome (`p[i]`)
- We keep expanding as long as characters match.

If this palindrome expands beyond `r`:
- We update `l` to be the new left boundary.
- We update `r` to be the new right boundary

Last step is to find the longest palindrome from `p` and return the substring.

TC -> `O( n )`
SC -> `O( n )`

```cpp title=Code
class Solution {
public:
    string preprocess(string s) {
        string ss = "#";
        for (char x : s) {
            ss += x;
            ss += '#';
        }
        return ss;
    }

    vector<int> manacher(string s) {
        int n = s.length();
        vector<int> p(n, 0);
        int l = 0;  // left boundary of rightmost palindrome so far
        int r = -1; // right boundary of rightmost palindrome so far

        for (int i = 0; i < n; i++) {
            if (i < r) {
                int mirror = l + r - i;
                p[i] = min(r - i, p[mirror]);
            }

            // Attempt to expand palindrome centered at i
            while (i - (1 + p[i]) >= 0 && i + (1 + p[i]) < n &&
                   s[i + (1 + p[i])] == s[i - (1 + p[i])])
                p[i]++;

            // if palindrome centered at i expands past r,
            // adjust rightmost palindrome so far based on expanded palindrome
            if (i + p[i] > r) {
                l = i - p[i];
                r = i + p[i];
            }
        }
        return p;
    }

    string longestPalindrome(string s) {
        string ss = preprocess(s);
        vector<int> P = manacher(ss);
        int maxLen = 0, centerIndex = 0;
        for (int i = 1; i < ss.length() - 1; i++) {
            if (P[i] > maxLen) {
                maxLen = P[i];
                centerIndex = i;
            }
        }
        return s.substr((centerIndex - maxLen) / 2, maxLen);
    }
};
```



##### Two Pointer Approach
[Video Explanation](https://youtu.be/XYQecbcd6_c )
1. We initialize `res_len` to keep track of the longest palindrome length and `si` for its starting index.
2. We iterate through each character in the string (index `i`).
3. For each character, we consider two types of palindromes: a) Odd-length palindromes b) Even-length palindromes
4. For odd-length palindromes:
    - We set two pointers, `l` and `r`, both initially at `i`.
    - We expand outwards (decrement `l`, increment `r`) as long as:
        - a) We're within string bounds
        -  b) Characters at `l` and `r` match
    - If we find a longer palindrome, we update `res_len` and `si`.
5. For even-length palindromes:
    - We set `l` to `i` and `r` to `i+1`.
    - We follow the same expansion process as for odd-length palindromes.
6. After checking both odd and even palindromes centered at each character, we move to the next character.
7. Once we've checked all characters, we return the substring starting at `si` with length `res_len`.

TC -> `O( n^2 )`
SC -> `O( 1 )`

```cpp title=Code
class Solution {
public:
    string longestPalindrome(string s) {
        int res_len = 0;
        int si = 0;

        for (int i = 0; i < s.size(); i++) {
            // odd length palindromes
            int l = i, r = i;
            while (l >= 0 && r < s.size() && s[l] == s[r]) {
                if ((r - l + 1) > res_len) {
                    res_len = r - l + 1;
                    si = l;
                }
                l--;
                r++;
            }

            // even length palindromes
            l = i, r = i + 1;
            while (l >= 0 && r < s.size() && s[l] == s[r]) {
                if ((r - l + 1) > res_len) {
                    res_len = r - l + 1;
                    si = l;
                }
                l--;
                r++;
            }
        }
        return s.substr(si, res_len);
    }
};
```


##### DP Tabulation Approach
[Video Explanation](https://youtu.be/UflHuQj6MVA)

To improve over the brute force solution, we first observe how we can avoid unnecessary re-computation while validating palindromes. Consider the case "ababa". If we already knew that "bab" is a palindrome, it is obvious that "ababa" must be a palindrome since the two left and right end letters are the same.

1. We initialize a boolean table `dp` and mark all the values as false. row-axis will be the starting indexes and column-axis will be the ending indexes.
2. We will use a variable `max_len`  to keep track of the maximum length of the palindrome and `si` to keep track of starting index of largest palindrome so far.
3. We will mark all the diagonal elements as true as every single character is a palindrome.
4. Now, we will iterate over the string and check for all substrings of length 2 separately because they're a base case for longer palindromes.
5. Then we will iterate over the string and check for substrings of length 3 or more. We will do this by using a nested loop.
	- The outer loop iterates over lengths from 3 to n.
	- The inner loop iterates over all possible starting positions for each length.
	- For each substring, we check if it's a palindrome by: 
			a) Checking if the first and last characters match
			b) Checking if the substring between them is already a palindrome (using the dp table)
1. We continually update max_len and si as we find longer palindromes.
2. Return the maximum substring.


TC -> `O( n^2 )`
SC -> `O( n^2 )`

```cpp title=Code
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();

        // edge case
        if (n <= 1)
            return s;
        // default answer if no palindrome of length 2 or greater found
        int max_len = 1, si = 0;

        vector<vector<bool>> dp(n, vector<bool>(n, false));

        // All substrings of length 1 are palindromes
        for (int i = 0; i < n; i++) {
            dp[i][i] = true;
        }

        // Check for substrings of length 2
        for (int i = 0; i < n - 1; i++) {
            if (s[i] == s[i + 1]) {
                dp[i][i + 1] = true;
                max_len = 2;
                si = i;
            }
        }

        // Check for lengths greater than 2
        for (int len = 3; len <= n; len++) {
            for (int i = 0; i < n - len + 1; i++) {
                int j = i + len - 1; // ending index
                if (s[i] == s[j] && dp[i + 1][j - 1]) {
                    dp[i][j] = true;
                    if (len > max_len) {
                        max_len = len;
                        si = i;
                    }
                }
            }
        }

        return s.substr(si, max_len);
    }
};
```


##### Brute Force Approach
Produce all possible substrings and check if it is a palindrome

TC -> `O( n^3 )`

