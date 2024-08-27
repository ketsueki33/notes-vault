---
difficulty: medium
leetcode-num: 650
topics:
  - Math
---
[Problem Link](https://leetcode.com/problems/2-keys-keyboard/)

#### Problem
There is only one character `'A'` on the screen of a notepad. You can perform one of two operations on this notepad for each step:

- Copy All: You can copy all the characters present on the screen (a partial copy is not allowed).
- Paste: You can paste the characters which are copied last time.

Given an integer `n`, return _the minimum number of operations to get the character_ `'A'` _exactly_ `n` _times on the screen_.

**Example 1:**

**Input:** n = 3
**Output:** 3
**Explanation:** Initially, we have one character 'A'.
In step 1, we use Copy All operation.
In step 2, we use Paste operation to get 'AA'.
In step 3, we use Paste operation to get 'AAA'.

**Example 2:**

**Input:** n = 1
**Output:** 0

**Constraints:**

- `1 <= n <= 1000`

#### Solution

##### Optimal Approach

*TC ->* O( `√n * log n` )
*SC ->* O( log n ) (recursion stack)

```cpp title=Code
class Solution {
    int smallestDivisor(int n) {
        // if divisible by 2
        if (n % 2 == 0)
            return 2;

        // iterate from 3 to sqrt(n)
        for (int i = 3; i * i <= n; i += 2) {
            if (n % i == 0)
                return i;
        }
        return n;
    }

public:
    int minSteps(int n) { 
        if(n==1 || n==0) return 0;

        int x =smallestDivisor(n);

        return x + minSteps(n/x);
     }
};
```