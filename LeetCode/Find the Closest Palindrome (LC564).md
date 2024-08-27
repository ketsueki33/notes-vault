---
difficulty: hard
leetcode-num: 564
topics:
  - String
  - Math
---
[Problem Link](https://leetcode.com/problems/find-the-closest-palindrome/)

#### Problem
Given a string `n` representing an integer, return _the closest integer (not including itself), which is a palindrome_. If there is a tie, return _**the smaller one**_.

The closest is defined as the absolute difference minimized between two integers.

**Example 1:**

**Input:** n = "123"
**Output:** "121"

**Example 2:**

**Input:** n = "1"
**Output:** "0"
**Explanation:** 0 and 2 are the closest palindromes but we return the smallest which is 0.

**Constraints:**

- `1 <= n.length <= 18`
- `n` consists of only digits.
- `n` does not have leading zeros.
- `n` is representing an integer in the range `[1, 1018 - 1]`.

#### Solution

For any possible number, there is 5 cases:  

Say the number is 4723

- Case 1. The next closest palidrome has one digit extra : So here it will be 10001
- Case 2. The next closest palindrome has one digit less: So here it will be 999
- Case 3. The next closest palidrome has the same number of digits . For case 3 there are 3 subcases:  
    1. The middle digit remains same.Postfix is the mirror image of prefix. So here 47(prefix)74(postfix)-->4774  
    2. The middle digit increases by 1.Postfix is the mirror image of prefix. So here 4884  
    3. The middle digit decreases by 1.Postfix is the mirror image of prefix. So here 4664  

Among these 5 candidates:  
The candidate having the least absolute difference from the original number is the answer. In this case it is 4774.

*TC ->* O( 1 )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    string nearestPalindromic(string n) {
        int digits = n.size();

        if (digits == 1)
            return to_string(stoi(n) - 1);

        vector<long> candidates;
        candidates.push_back(pow(10, digits) + 1);     // Case 1
        candidates.push_back(pow(10, digits - 1) - 1); // Case 2

        // Case 3
        int mid = (digits + 1) / 2;
        long prefix = stol(n.substr(0, mid));

        vector<long> v = {prefix, prefix + 1, prefix - 1};
        for (long x : v) {
            string postfix = to_string(x);
            if (digits % 2 != 0)
                postfix.pop_back(); // for odd numbers
            reverse(postfix.begin(), postfix.end());
            string s = to_string(x) + postfix;
            candidates.push_back(stol(s));
        }

        long mindiff = LONG_MAX, res, num = stol(n);

        for (long x : candidates) {
            long diff = abs(x - num);
            if (x != num && diff < mindiff) // ans cant be same as given number
            {
                mindiff = diff;
                res = x;
            } else if (diff == mindiff)
                res = min(res, x);
        }
        return to_string(res);
    }
};
```