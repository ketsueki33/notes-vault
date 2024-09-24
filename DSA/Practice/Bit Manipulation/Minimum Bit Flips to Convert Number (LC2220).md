---
difficulty: easy
leetcode-num: 2220
topics:
  - Bit Manipulation
---
[Problem Link](https://leetcode.com/problems/minimum-bit-flips-to-convert-number/)

#### Problem
A **bit flip** of a number `x` is choosing a bit in the binary representation of `x` and **flipping** it from either `0` to `1` or `1` to `0`.

- For example, for `x = 7`, the binary representation is `111` and we may choose any bit (including any leading zeros not shown) and flip it. We can flip the first bit from the right to get `110`, flip the second bit from the right to get `101`, flip the fifth bit from the right (a leading zero) to get `10111`, etc.

Given two integers `start` and `goal`, return _the **minimum** number of **bit flips** to convert_ `start` _to_ `goal`.

**Example 1:**

**Input:** start = 10, goal = 7
**Output:** 3
**Explanation:** The binary representation of 10 and 7 are 1010 and 0111 respectively. We can convert 10 to 7 in 3 steps:
- Flip the first bit from the right: 1010 -> 1011.
- Flip the third bit from the right: 1011 -> 1111.
- Flip the fourth bit from the right: 1111 -> 0111.
It can be shown we cannot convert 10 to 7 in less than 3 steps. Hence, we return 3.

**Example 2:**

**Input:** start = 3, goal = 4
**Output:** 3
**Explanation:** The binary representation of 3 and 4 are 011 and 100 respectively. We can convert 3 to 4 in 3 steps:
- Flip the first bit from the right: 011 -> 010.
- Flip the second bit from the right: 010 -> 000.
- Flip the third bit from the right: 000 -> 100.
It can be shown we cannot convert 3 to 4 in less than 3 steps. Hence, we return 3.

**Constraints:**

- `0 <= start, goal <= 109`

#### Solution

We will do a XOR of `start` and `goal`. This will give us a number in which bits that are `1` are the bits that are different in `start` and `goal` and hence need to be flipped.  We will then just count the number of `1` bits in `num`. That will be our answer.

The solution uses a loop to count how many `1`s are present in `num`. For each iteration, it checks whether the least significant bit (`num & 1`) is `1` (indicating a difference between `start` and `goal` at that bit position). If so, it increments the `flipCount`.

After checking each bit, the number is right-shifted (`num >>= 1`) to move to the next bit position until all bits have been checked (i.e., until `num` becomes `0`).

*TC ->* O( log n )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    int minBitFlips(int start, int goal) {
        int flipCount = 0;

        int num = (start ^ goal);

        while (num) {
            if ((num & 1) == 1)
                flipCount++;
            num >>= 1;
        }

        return flipCount;
    }
};
```