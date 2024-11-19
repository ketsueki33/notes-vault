---
difficulty: hard
leetcode-num: 214
topics:
  - String
---
[Problem Link](https://leetcode.com/problems/shortest-palindrome/)

#### Problem
You are given a string `s`. You can convert `s` to a palindrome by adding characters in front of it.

Return _the shortest palindrome you can find by performing this transformation_.

**Example 1:**

**Input:** s = "aacecaaa"
**Output:** "aaacecaaa"

**Example 2:**

**Input:** s = "abcd"
**Output:** "dcbabcd"

**Constraints:**

- `0 <= s.length <= 5 * 104`
- `s` consists of lowercase English letters only.

#### Solution
This problem is essentially about finding the longest palindromic substring starting at index 0.
There also exists a KMP Algorithm based solution.

##### KMP Approach (todo)
[video link](https://youtu.be/5DACQK9kud0)
that would make this a question of `string matching`.
##### Manacher's Algorithm Approach
We can use Manacher's Algorithm to optimise the process of finding the `farthestCenter` in the [[#Brute Force Approach (TLE)|Brute Force Approach]]. Refer this for a detailed working of [[Longest Palindromic Substring (LC5)#Manacher's Algorithm|Manacher's Algorithm]]

Solution is similar to the Brute Force Approach.. the difference is that we are using Manacher's algorithm to find the longest palindromic prefix. After running Manacher's Algorithm, we look for the palindrome that starts at the beginning of the string. The farthest center of such a palindrome is recorded.


*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {
    // Preprocess the string to insert '#' between every character and at the
    // start and end
    string preProcess(const string& s) {
        string ss = "#";
        for (auto& ch : s) {
            ss += ch;
            ss += "#";
        }
        return ss;
    }

public:
    string shortestPalindrome(string s) {
        // Step 1: Preprocess the string to add special characters
        string ss = preProcess(s);
        int n = ss.size();

        // Step 2: Manacher's Algorithm
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
                   ss[i + (1 + p[i])] == ss[i - (1 + p[i])])
                p[i]++;

            // if palindrome centered at i expands past r, adjust rightmost
            // palindrome so far based on expanded palindrome
            if (i + p[i] > r) {
                l = i - p[i];
                r = i + p[i];
            }
        }

        // Step 3: Find the longest palindromic prefix
        int farthestCenter = 0;
        for (int i = 0; i < n; ++i) {
            // Check if the palindrome centered at i extends to the start
            // (i.e.,is a prefix)

            if (i - p[i] == 0) {
                farthestCenter = p[i];
            }
        }

        // Step 4: Add characters to the front to make the string a palindrome
        string extra = ss.substr((farthestCenter * 2) + 1);
        reverse(ss.begin(), ss.end());

        ss += extra;

        // Step 5: remove extra '#' and return the result
        string res;
        for (auto& ch : ss)
            if (ch != '#')
                res += ch;

        return res;
    }
};
```

##### Brute Force Approach (TLE)

- **Preprocessing the String**:
    - The function `preProcess` converts the string `s` into a new string where each character is separated by `#`, and `#` is also added at the start and end.
    - This is done to uniformly handle both even-length and odd-length palindromes.
    - For example, if `s = "abc"`, the preprocessed string becomes `ss = "#a#b#c#"`. This helps in simplifying palindrome checking.
- **Identifying the Longest Palindromic Prefix**:    
    - The main goal is to find the **longest palindromic prefix** (the longest part of the string starting from the first character that is a palindrome).
    - The variable `farthestCenter` is initialized to the center of the string `ss` and is used to track the farthest center of a palindrome that extends to the start of the string.
- **Iterating Backwards from the Center**:    
    - The for-loop iterates backward from the center of the string, trying to find the largest palindrome centered at `i`.
    - The inner while-loop checks if the characters on both sides of the center (`ss[i - j]` and `ss[i + j]`) are equal. If they are, it expands the palindrome by incrementing `j`.
    - If the palindrome extends to the very start of the string (i.e., `i - j + 1 == 0`), it breaks out of the loop, and the center of this palindrome is stored in `farthestCenter`.
- **Forming the Shortest Palindrome**:    
    - The `extra` string is the part of `ss` that needs to be mirrored and added to the start to make the string a palindrome. This is extracted using `ss.substr((farthestCenter * 2) + 1)`, which gets the part of the string starting after the longest palindromic prefix.
    - The original string `ss` is reversed, and then `extra` is appended to it.
- **Removing `#` Characters**:    
    - Since the preprocessed string has `#` characters, we loop through the combined string to construct the final result by skipping all `#` characters.
- **Returning the Result**:    
    - The resulting string is returned, which is the **shortest palindrome** formed by adding the minimal number of characters to the start of the original string.

*TC ->* O( n$^{2}$ )
*SC ->* O( n )

```cpp title=Code
class Solution {
    string preProcess(string& s) {
        string ss = "#";

        for (auto& x : s) {
            ss += x;
            ss += "#";
        }
        return ss;
    }

public:
    string shortestPalindrome(string s) {
        string ss = preProcess(s);
        int n = ss.size();

        int farthestCenter = n / 2;

        for (int i = farthestCenter; i >= 0; i--) {
            int j = 1;
            while (i - j >= 0 && ss[i - j] == ss[i + j]) {
                j++;
            }

            if ((i - j + 1) == 0) {
                farthestCenter = i;
                break;
            }
        }

        string extra = ss.substr((farthestCenter * 2) + 1);
        reverse(ss.begin(), ss.end());

        ss += extra;

        string res;
        for (auto& ch : ss)
            if (ch != '#')
                res += ch;

        return res;
    }
};
```