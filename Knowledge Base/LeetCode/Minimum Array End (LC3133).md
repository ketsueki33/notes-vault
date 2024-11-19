---
difficulty: medium
leetcode-num: 3133
topics:
  - Bit Manipulation
---
 [Problem Link](https://leetcode.com/problems/minimum-array-end/)

#### Problem
You are given two integers `n` and `x`. You have to construct an array of **positive** integers `nums` of size `n` where for every `0 <= i < n - 1`, `nums[i + 1]` is **greater than** `nums[i]`, and the result of the bitwise `AND` operation between all elements of `nums` is `x`.

Return the **minimum** possible value of `nums[n - 1]`.

**Example 1:** 

**Input:** n = 3, x = 4

**Output:** 6

**Explanation:**

`nums` can be `[4,5,6]` and its last element is 6.

**Example 2:**

**Input:** n = 2, x = 7

**Output:** 15

**Explanation:**

`nums` can be `[7,15]` and its last element is 15.

**Constraints:**

- `1 <= n, x <= 108`

#### Solution
##### Optimal Approach

*TC ->* O( `log n` )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    long long minEnd(int n, int x) {
        n--;
        long long a = x, b;
        for (b = 1; n > 0; b <<= 1) {
            if ((b & x) == 0) {
                a |= (n & 1) * b;
                n >>= 1;
            }
        }
        return a;
    }
};class Solution {
public:
    long long minEnd(int n, int x) {
        n--;
        long long a = x, b;
        for (b = 1; n > 0; b <<= 1) {
            if ((b & x) == 0) {
                a |= (n & 1) * b;
                n >>= 1;
            }
        }
        return a;
    }
};
```

##### Brute Force Approach - O(n)
[Video Explanation](https://youtu.be/rChLZzzggjo)

- **Initialize with `x`**: The variable `num` is initialized to `x`, as all elements in the array need to maintain an `AND` result that includes `x`.
    
- **Bitwise OR Adjustment**: For each position `i` (from 1 to `n-1`), `num` is incremented by 1 and then adjusted by applying the bitwise `OR` with `x`. This operation guarantees that each new number is greater than the previous one and maintains the bitwise `AND` requirements.
    
    - The bitwise `OR` ensures that all bits required to match `x` remain present in `num` as it grows.
- **Return Result**: After iterating `n-1` times, `num` represents the minimum possible value for the last element in the array, which meets the problem's conditions.

*TC ->* O( n )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    long long minEnd(int n, int x) {
        long long num = x;

        for( int i = 1 ; i < n ; i++ ){
            num = (num+1)|x;
        }

        return num;
    }
};
```

##### Brute Force Approach - O(x)
- **Initialize Variables**:
    
    - `cand`: A vector to store candidate values that could be used in the array.
    - `temp`: A temporary variable initialized to `x`, which will be incremented to find candidate values.
    - `digits`: The number of binary digits (or bits) in `x`. This is calculated as `log2(x) + 1`.
- **Identify Candidates (`cand` Vector)**:
    
    - The `while` loop finds all candidate values that satisfy `(temp & x) == x` as `temp` increments from `x`.
    - This condition ensures that each candidate in `cand` has all the bits of `x` in the `AND` operation with `temp`. Only values that "keep" `x` when `AND`ed are stored as candidates.
    - The loop continues until `(temp & x) == 0`, which indicates that `temp` no longer shares enough bits with `x` for a meaningful `AND` result.
    - After this loop, `cand` will contain all possible values starting from `x` that can satisfy the `AND` condition with `x`.
- **Determine the Minimum `nums[n-1]`**:
    
    - `prefix` is calculated as `(n-1) / cand.size()`, which represents how many "sets" of `cand` values would fit into the sequence leading up to the last element.
    - `idx` is calculated as `(n-1) % cand.size()`, which determines the exact position in `cand` for the last element.
- **Calculate `nums[n-1]`**:
    
    - `prefix << digits`: Shifts `prefix` by the number of bits in `x`. This multiplication ensures that `prefix` takes its position as the "prefix" in the binary form for `nums[n-1]`.
    - `+ cand[idx]`: Adds the appropriate candidate value from `cand` at index `idx`, finalizing `nums[n-1]` as the smallest possible value that satisfies all conditions.
- **Return the Result**:
    
    - `nums[n-1]` is returned as the result, which is the smallest possible last element that makes the array strictly increasing and gives an `AND` result of `x`.

*TC ->* O( x )
*SC ->* O( x )

```cpp title=Code
class Solution {
public:
    long long minEnd(int n, int x) {
        vector<long long> cand;

        long long temp = x;
        int digits = log2(x) + 1;

        while ((temp & x) != 0) {
            if ((temp & x) == x)
                cand.push_back(temp);
            temp++;
        }

        long long prefix = (n - 1) / cand.size();
        int idx = (n - 1) % cand.size();

        return (prefix << digits) + cand[idx];
    }
};
```