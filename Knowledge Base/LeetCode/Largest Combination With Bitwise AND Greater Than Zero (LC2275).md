---
difficulty: medium
leetcode-num: 2275
topics:
  - Array
  - Bit Manipulation
  - Hash Table
  - Counting
---
[Problem Link](https://leetcode.com/problems/largest-combination-with-bitwise-and-greater-than-zero/)

#### Problem
The **bitwise AND** of an array `nums` is the bitwise AND of all integers in `nums`.

- For example, for `nums = [1, 5, 3]`, the bitwise AND is equal to `1 & 5 & 3 = 1`.
- Also, for `nums = [7]`, the bitwise AND is `7`.

You are given an array of positive integers `candidates`. Evaluate the **bitwise AND** of every **combination** of numbers of `candidates`. Each number in `candidates` may only be used **once** in each combination.

Return _the size of the **largest** combination of_ `candidates` _with a bitwise AND **greater** than_ `0`.

**Example 1:**

**Input:** candidates = [16,17,71,62,12,24,14]
**Output:** 4
**Explanation:** The combination [16,17,62,24] has a bitwise AND of 16 & 17 & 62 & 24 = 16 > 0.
The size of the combination is 4.
It can be shown that no combination with a size greater than 4 has a bitwise AND greater than 0.
Note that more than one combination may have the largest size.
For example, the combination [62,12,24,14] has a bitwise AND of 62 & 12 & 24 & 14 = 8 > 0.

**Example 2:**

**Input:** candidates = [8,8]
**Output:** 2
**Explanation:** The largest combination [8,8] has a bitwise AND of 8 & 8 = 8 > 0.
The size of the combination is 2, so we return 2.

**Constraints:**

- `1 <= candidates.length <= 105`
- `1 <= candidates[i] <= 107`

#### Solution
[Video Explanation](https://youtu.be/9ZlMnn_N0QE)

##### Optimal Approach
###### Key Concepts

1. **Bit Shifting**: `(1 << i)` shifts the bit `1` left by `i` positions, resulting in a mask where only the `i`th bit is `1` and all other bits are `0`.
2. **Bitwise AND**: `(num & (1 << i))` checks if the `i`th bit in `num` is `1`. If it is, the result is non-zero; otherwise, it’s zero.

###### Steps in the Code

1. **Outer Loop for Each Bit Position**:
    
    - `for (int i = 0; i < 24; i++)` iterates over bit positions from `0` to `23`.
    - `24` is chosen because all numbers fit within a 24-bit representation according to the problem constraints.
2. **Count `1`s in Each Bit Position Across All Numbers**:
    
    - For each bit position `i`, initialize `count` to `0`. This will keep track of the count of `1`s for the `i`th bit across all numbers in `candidates`.
    - For each number `num` in `candidates`:
        - The expression `(num & (1 << i)) != 0` checks if the `i`th bit in `num` is `1`.
        - If true, increment `count`.
    - After processing all numbers for a particular `i`, `count` will contain the number of `1`s in the `i`th bit position across all numbers.
3. **Update Maximum Count**:
    
    - After calculating `count` for the current bit position `i`, update `maxCount` with the maximum of `count` and `maxCount`.
    - This ensures that `maxCount` will store the highest count of `1`s found in any bit position by the end of the loops.
4. **Return Result**:
    
    - `maxCount` is returned as the largest combination, which represents the maximum number of numbers that have a specific bit position set to `1`.

*TC ->* O( `24 * n` )
*SC ->* O( 1 )

```cpp title=Code
class Solution {

public:
    int largestCombination(vector<int>& candidates) {
        int n = candidates.size();
        int maxCount = 0;

        for (int i = 0; i < 24; i++) {
            int count = 0;

            for (int num : candidates)
                if ((num & (1 << i)) != 0)
                    count++;

            maxCount = max(count, maxCount);
        }

        return maxCount;
    }
};
```

##### Alright Approach (Extra Array)
###### Key Variables and Functions

1. **`bitCount`**: A vector of size 24 initialized to 0, used to store the count of `1`s for each bit position from the least significant to the 23rd bit position (24 bits are sufficient for the largest possible number in the `candidates` vector according to the problem constraints).
    
2. **`findBits(int num)`**: This helper function processes a single integer (`num`) and updates the `bitCount` vector based on the bit positions that are `1` in `num`.


###### Detailed Steps

1. **Counting `1`s at Each Bit Position**:
    
    - For each number in the `candidates` vector, `findBits` is called.
    - `findBits` uses a `while` loop to check each bit in `num`:
        - If the current bit (from the right) is `1`, it increments the corresponding position in `bitCount`.
        - `bitIdx` keeps track of the current bit position.
        - The `num >>= 1;` operation shifts `num` one bit to the right, moving to the next bit in the binary representation.
2. **Updating the Maximum Count**:
    
    - After counting all bits in all numbers, the `bitCount` vector contains the counts of `1`s at each bit position across all numbers.
    - The outer loop iterates over each element in `bitCount`, updating `maxCount` to store the highest count found in any bit position.
3. **Returning the Result**:
    
    - `maxCount` represents the largest combination of bits that are `1` across all numbers, so this value is returned.

*TC ->* O( `24 * n` )
*SC ->* O( 24 )

```cpp title=Code
class Solution {
    vector<int> bitCount = vector<int>(24, 0);

    void findBits(int num) {
        int bitIdx = 0;

        while (num) {
            if ((num & 1) == 1)
                bitCount[bitIdx]++;

            bitIdx++;
            num >>= 1;
        }
    }

public:
    int largestCombination(vector<int>& candidates) {
        int n = candidates.size();
        int maxCount = 0;

        for (int x : candidates)
            findBits(x);

        for (int x : bitCount)
            maxCount = max(x, maxCount);

        return maxCount;
    }
};
```