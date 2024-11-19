---
difficulty: easy
leetcode-num: 476
topics:
  - Bit Manipulation
---
[Problem Link](https://leetcode.com/problems/number-complement/)

#### Problem
The **complement** of an integer is the integer you get when you flip all the `0`'s to `1`'s and all the `1`'s to `0`'s in its binary representation.

- For example, The integer `5` is `"101"` in binary and its **complement** is `"010"` which is the integer `2`.

Given an integer `num`, return _its complement_.

**Example 1:**

**Input:** num = 5
**Output:** 2
**Explanation:** The binary representation of 5 is 101 (no leading zero bits), and its complement is 010. So you need to output 2.

**Example 2:**

**Input:** num = 1
**Output:** 0
**Explanation:** The binary representation of 1 is 1 (no leading zero bits), and its complement is 0. So you need to output 0.

**Constraints:**

- `1 <= num < 231`

#### Solution


##### Optimal Approach
[Video Explanation](https://youtu.be/LA1BnKiarEQ)

We initialize two variables: 
- `res`: to store the answer
- `i`: to keep track of the current digit place

Then we iterate in the loop till `num` is not 0:
- If bitmask with 1 is equal to 0.. it means the binary digit is 0. So it's complement will be 1. This means we add it to the result. **Before adding, we left shift by `i` to account for the digit place** (eg. If its 3rd digit from right, then we will add 4 which is 1 left shifted by 2 ).
- right shift `num` by 1 to remove the processed digit  place.
- increment `i`.

*TC ->* O( log n  )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    int findComplement(int num) {
        int res = 0;
        int i = 0;
        while (num) {
            if ((num & 1) == 0)
                res = res + (1 << i);
            num >>= 1;
            i++;
        }
        return res;
    }
};
```