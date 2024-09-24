---
difficulty: medium
leetcode-num: 1371
topics:
  - Bit Manipulation
  - Hash Table
  - Prefix Sum
  - String
---
[Problem Link](https://leetcode.com/problems/find-the-longest-substring-containing-vowels-in-even-counts/description/)

#### Problem
Given the string `s`, return the size of the longest substring containing each vowel an even number of times. That is, 'a', 'e', 'i', 'o', and 'u' must appear an even number of times.

**Example 1:**

**Input:** s = "eleetminicoworoep"
**Output:** 13
**Explanation:** The longest substring is "leetminicowor" which contains two each of the vowels: **e**, **i** and **o** and zero of the vowels: **a** and **u**.

**Example 2:**

**Input:** s = "leetcodeisgreat"
**Output:** 5
**Explanation:** The longest substring is "leetc" which contains two e's.

**Example 3:**

**Input:** s = "bcbcbc"
**Output:** 6
**Explanation:** In this case, the given string "bcbcbc" is the longest because all vowels: **a**, **e**, **i**, **o** and **u** appear zero times.

**Constraints:**
- `1 <= s.length <= 5 x 10^5`
- `s` contains only lowercase English letters.

#### Solution
[Video Explanation](https://youtu.be/6Xf5LfM-ciI)
###### Why is Sliding Window not feasible?
 The logic to shrink the sliding window will not work in this problem. If we encounter a vowel.. it's count may be odd now but later the count could become even. There is no way to account for that in the sliding window.
###### Our Approach
We will be using bitmasking along with a prefix sum like approach. 

1. We maintain a **bitmask** that tracks the parity (odd/even count) of vowels in the substring up to the current index.
2. If the bitmask repeats, it means the substring between these two indices contains vowels in even counts. This is similar to how when a prefix sum repeats it means that the subarray sum is 0.

- The bitmask here is a 5-bit binary number where each bit represents whether a vowel has been encountered an odd or even number of times:
    - 1st bit: `a`
    - 2nd bit: `e`
    - 3rd bit: `i`
    - 4th bit: `o`
    - 5th bit: `u`
    - For each vowel, toggling its respective bit changes the parity (odd ↔ even).

*TC ->* O( n )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    int findTheLongestSubstring(string s) {
        unordered_map<int,int> mp;
        int res = 0;
        int mask = 0; //00000
        mp[mask] = -1;

        for( int i = 0 ; i < s.size() ; i++){
            char ch = s[i];

            if( ch == 'a' )
                mask = (mask^(1<<0));
            else if(ch == 'e')
                mask = (mask^(1<<1));
            else if(ch == 'i')
                mask = (mask^(1<<2));
            else if(ch == 'o')
                mask = (mask^(1<<3));
            else if(ch == 'u')
                mask = (mask^(1<<4));

            if( mp.count(mask) > 0 )
                res = max(res, i - mp[mask]);
            else
                mp[mask] = i;
        }
        return res;
    }
};
```