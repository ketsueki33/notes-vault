---
difficulty: medium
leetcode-num: 567
topics:
  - Hash Table
  - Two Pointer
  - String
  - Sliding Window
---
[Problem Link](https://leetcode.com/problems/permutation-in-string/)

#### Problem
Given two strings `s1` and `s2`, return `true` if `s2` contains a

permutation

of `s1`, or `false` otherwise.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

**Example 1:**

**Input:** s1 = "ab", s2 = "eidbaooo"
**Output:** true
**Explanation:** s2 contains one permutation of s1 ("ba").

**Example 2:**

**Input:** s1 = "ab", s2 = "eidboaoo"
**Output:** false

**Constraints:**

- `1 <= s1.length, s2.length <= 104`
- `s1` and `s2` consist of lowercase English letters.

#### Solution
[Video Explanation](https://youtu.be/iTwwvsyUsi4)

##### Sliding Window with Hash(Optimal Approach)
###### Key Concepts:

1. **Frequency Arrays**:
    
    - **`freq`**: This array tracks the frequency of characters in `s1`. Since there are only 26 lowercase English letters, we use an array of size 26.
    - **`check`**: This array tracks the frequency of characters in the current window of `s2`.
2. **Sliding Window**:
    
    - We want to compare the frequency of characters in a window of `s2` (same size as `s1`) to the frequency of characters in `s1`.
    - The window slides through `s2`, one character at a time.

###### Step-by-Step Breakdown:
1. **Edge Case Check**:
    
    - If `s1` is larger than `s2`, it's impossible for a permutation of `s1` to be a substring of `s2`, so we return `false`.
2. **Initialize `freq` for `s1`**:
    
    - We fill the `freq` array with the frequency of characters in `s1`. For each character `ch` in `s1`, we increment `freq[ch - 'a']`. The expression `ch - 'a'` gives the index corresponding to the character in the alphabet (e.g., `'a'` is index 0, `'b'` is index 1, etc.).
3. **Initialize `check` for the first window of `s2`**:
    
    - We create the initial frequency count for the first `s1.size()` characters of `s2` and store it in `check`.
4. **Slide the window through `s2`**:
    
    - Start at the position where the first window ends (`s1.size() - 1`). For each character in `s2`, we compare the frequency arrays:
        - **If it's the first window**, the `check` array is already initialized.
        - **For subsequent windows**, we adjust the `check` array by:
            - **Adding** the character at the end of the new window (`s2[i]`).
            - **Removing** the character at the start of the previous window (`s2[i - s1.size()]`).
        - After each adjustment, if `check` is equal to `freq`, it means we've found a valid permutation of `s1` in `s2`.
5. **Return the result**:
    
    - If a matching window is found, return `true`.
    - If the loop completes without finding any matches, return `false`.

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {

public:
    bool checkInclusion(string s1, string s2) {
        vector<int> freq(26, 0), check(26, 0);

        if (s1.size() > s2.size())
            return false;

        for (char ch : s1)
            freq[ch - 'a']++;

        for (int i = 0; i < s1.size(); i++) {
            check[s2[i] - 'a']++;
        }

        for (int i = s1.size() - 1; i < s2.size(); i++) {
            if (i != s1.size() - 1) {
                check[s2[i] - 'a']++;
                check[s2[i - s1.size()] - 'a']--;
            }

            if (freq == check)
                return true;
        }

        return false;
    }
};
```


##### Brute Force (TLE)
We will generate all permutations and store them in a unordered set. Then we will check every substring in`s2` of size(`s1`), if it exists in the map. If it does, we will return `true` otherwise `false`.

*TC ->* O( n! )
*SC ->* O( n! )

```cpp title=Code
class Solution {
    unordered_set<string> st;

    void generatePermutations(string& s, int index = 0) {
        if (index == s.size()) {
            st.insert(s);
            return;
        }

        for (int i = index; i < s.size(); i++) {
            swap(s[index], s[i]);
            generatePermutations(s, index + 1);
            swap(s[index], s[i]);
        }
    }

public:
    bool checkInclusion(string s1, string s2) {
        int windowSize = s1.size();

        generatePermutations(s1);

        if (s1.size() > s2.size())
            return false;

        for (int i = 0; i <= s2.size() - windowSize; i++) {
            if (st.count(s2.substr(i, windowSize))) {
                return true;
            }
        }
        return false;
    }
};
```
