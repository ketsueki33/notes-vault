---
difficulty: medium
leetcode-num: 1545
topics:
  - String
  - Recursion
  - Simulation
---
[Problem Link](https://leetcode.com/problems/find-kth-bit-in-nth-binary-string)

#### Problem

Given two positive integers `n` and `k`, the binary string `Sn` is formed as follows:

- `S1 = "0"`
- `Si = Si - 1 + "1" + reverse(invert(Si - 1))` for `i > 1`

Where `+` denotes the concatenation operation, `reverse(x)` returns the reversed string `x`, and `invert(x)` inverts all the bits in `x` (`0` changes to `1` and `1` changes to `0`).

For example, the first four strings in the above sequence are:

- `S1 = "0"`
- `S2 = "0**1**1"`
- `S3 = "011**1**001"`
- `S4 = "0111001**1**0110001"`

Return _the_ `kth` _bit_ _in_ `Sn`. It is guaranteed that `k` is valid for the given `n`.

**Example 1:**

**Input:** n = 3, k = 1
**Output:** "0"
**Explanation:** S3 is "**0**111001".
The 1st bit is "0".

**Example 2:**

**Input:** n = 4, k = 11
**Output:** "1"
**Explanation:** S4 is "0111001101**1**0001".
The 11th bit is "1".

**Constraints:**

- `1 <= n <= 20`
- `1 <= k <= 2n - 1`
#### Solution
##### Optimal Approach

The solution leverages recursion to avoid generating the entire string. Instead, it uses the properties of the string’s construction.

###### Key Points
- **String Length**: The total length of the string at level `n` is `2^n - 1`. This is key to determining the midpoint.
- **Midpoint**: The middle element is always "1". If `k` equals the midpoint, the result is immediately known.
- **Recursive Substructure**: 
  - If `k` is less than the midpoint, we can look in the first half, which corresponds to `S_(n-1)`.
  - If `k` is greater than the midpoint, it’s equivalent to looking in the reverse-inverted `S_(n-1)`.


> [!info] First Half is always non-inverted
> No matter what happens at higher levels of recursion, the first half of each string `S_n` is always the same as the previous string `S_(n-1)` without any inversion. This is because:
> 
> - The structure of the string is `S_(n-1) + "1" + reverse-inverted(S_(n-1))`.
> - Only the second half (right side) is the reverse-inverted portion.
> 
> So, when `k` falls within the first half (i.e., `k < mid`), the function does not invert, and we recurse with `inverted = false`.

###### Recursive Steps
1. **Base Case**: If `n == 1`, return `'0'` if `inverted` is false, else return `'1'`.
2. **Determine the Middle Element**:
   - `total = 2^n - 1`: This is the total length of the string.
   - `mid = total / 2 + 1`: This is the middle element.
3. **Check if `k` is at the Middle**:
   - If `k == mid`, return `'1'` if `inverted` is false, else return `'0'`.
4. **Recursive Call**:
   - If `k < mid`, recurse into `S_(n-1)` without inversion.
   - If `k > mid`, recurse into the reverse-inverted part of `S_(n-1)` with the index adjusted as `k - mid` and toggle the `inverted` flag.


*TC ->* O( n )
*SC ->* O( n ) (recursion stack)

```cpp title=Code
class Solution {

public:
    char findKthBit(int n, int k, bool inverted = false) {

        if (n == 1)
            return inverted ? '1' : '0';

        int total = pow(2, n) - 1 ;
        int mid = total / 2 + 1;

        if (k == mid)
            return inverted ? '0' : '1';

        if (k < mid)
            return findKthBit(n - 1, k, false);

        return findKthBit(n - 1, k - mid, true);
    }
};
```

##### Brute Force Approach

*TC ->* O( 2$^{n}$ )
*SC ->* O( 2$^{n}$  )

```cpp title=Code
class Solution {
    string reverseInvert(string s) {
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '0')
                s[i] = '1';
            else
                s[i] = '0';
        }
        reverse(s.begin(), s.end());

        return s;
    }

public:
    char findKthBit(int n, int k) {
        string s = "0";

        for (int i = 0; i < n; i++) {
            s += "1" + reverseInvert(s);
        }

        return s[k - 1];
    }
};
```