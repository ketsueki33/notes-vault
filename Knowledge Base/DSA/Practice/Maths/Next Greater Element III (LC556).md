---
difficulty: medium
leetcode-num: 556
topics:
  - String
  - Math
  - Two Pointer
---
[Problem Link](https://leetcode.com/problems/next-greater-element-iii/)

#### Problem
Given a positive integer `n`, find _the smallest integer which has exactly the same digits existing in the integer_ `n` _and is greater in value than_ `n`. If no such positive integer exists, return `-1`.

**Note** that the returned integer should fit in **32-bit integer**, if there is a valid answer but it does not fit in **32-bit integer**, return `-1`.

**Example 1:**

**Input:** n = 12
**Output:** 21

**Example 2:**

**Input:** n = 21
**Output:** -1

**Constraints:**

- `1 <= n <= 2^31 - 1`

#### Solution
##### Optimal Approach

> [!info] Next Permutationm
> The solution to this problem is very similar to [[../Arrays/Next Permutation (LC31)#Optimal Approach|Next Permutation (LC31)]]. See that for the detailed breakdown.
###### Explanation
1. **Convert the integer `n` to a string**:
    
    - The integer `n` is converted to a string `s` so that its digits can be manipulated easily.
2. **Find the first decreasing digit from the end**:
    
    - A variable `low` is used to track the first position from the right where a digit is **smaller than the digit next to it**. This identifies the point where the number stops increasing from right to left.
    - This step is crucial because, in order to find the next greater number, we need to increase the digit at `low` and rearrange the digits after it to form the smallest possible number.
    - If no such `low` is found (i.e., the digits are in non-increasing order), it means the number is already the largest possible permutation, so the function returns `-1`.
3. **Find the smallest digit larger than `s[low]`**:
    
    - Next, starting from the rightmost digit (`high = len-1`), we find the smallest digit that is larger than `s[low]`. This ensures that we are forming the smallest possible next permutation.
    - Once found, we swap `s[low]` with this digit.
4. **Reverse the digits after `low`**:
    
    - After the swap, the digits after the `low` position are guaranteed to be in decreasing order. Reversing them gives the smallest possible number with those digits, which ensures we get the **next lexicographical permutation**.
5. **Convert the string back to an integer**:
    
    - After forming the next permutation, the string `s` is converted back to a number using `stol`.
    - However, if the result exceeds `INT_MAX` (which is 2,147,483,647), it is invalid for a 32-bit signed integer, so the function returns `-1`.
6. **Return the result**:
    
    - If the result is valid and within the range of a 32-bit integer, it returns that value.

###### Key Steps:
- **Finding `low`**: This ensures that you identify the point where the digits are decreasing from right to left, indicating the last possible point to modify for a "next greater" number.
- **Swapping**: Ensures that we increase the number by the smallest possible value.
- **Reversing**: Ensures that after increasing, the remaining digits are arranged in the smallest possible order to maintain the next lexicographical permutation.

*TC ->* O( n )
*SC ->* O( n ), to store the string

```cpp title=Code
class Solution {
public:
    int nextGreaterElement(int n) {
        string s = to_string(n);

        int len = s.size(), low;

        for (low = len - 2; low >= 0; low--)
            if (s[low] < s[low + 1])
                break;

        if (low < 0)
            return -1;

        for (int high = len - 1; high >= 0; high--)
            if (s[high] > s[low]) {
                swap(s[high], s[low]);
                break;
            }

        reverse(s.begin() + low + 1, s.end());

        long res = stol(s);

        return res > INT_MAX ? -1 : res;
    }
};
```

#### Similar Problems
- [[../Arrays/Next Permutation (LC31)|Next Permutation (LC31)]]