---
difficulty: medium
leetcode-num: 2938
topics:
  - String
  - Two Pointer
  - Greedy
---
[Problem Link](https://leetcode.com/problems/separate-black-and-white-balls/)

#### Problem
There are `n` balls on a table, each ball has a color black or white.

You are given a **0-indexed** binary string `s` of length `n`, where `1` and `0` represent black and white balls, respectively.

In each step, you can choose two adjacent balls and swap them.

Return _the **minimum** number of steps to group all the black balls to the right and all the white balls to the left_.

**Example 1:**

**Input:** s = "101"
**Output:** 1
**Explanation:** We can group all the black balls to the right in the following way:
- Swap s[0] and s[1], s = "011".
Initially, 1s are not grouped together, requiring at least 1 step to group them to the right.

**Example 2:**

**Input:** s = "100"
**Output:** 2
**Explanation:** We can group all the black balls to the right in the following way:
- Swap s[0] and s[1], s = "010".
- Swap s[1] and s[2], s = "001".
It can be proven that the minimum number of steps needed is 2.

**Example 3:**

**Input:** s = "0111"
**Output:** 0
**Explanation:** All the black balls are already grouped to the right.

**Constraints:**

- `1 <= n == s.length <= 105`
- `s[i]` is either `'0'` or `'1'`.

#### Solution

##### Simple Count Approach
[Video Explanation](https://youtu.be/E6AKLOdt9jc)

###### Key Variables:

1. **res (result)**: This variable stores the total number of steps needed.
2. **blackCount**: This variable tracks how many '1's (referred to as "black" here) have been encountered as the string is traversed.

###### Approach:

1. **Iterating through the string**:
    
    - The solution uses a loop to traverse the string `s` character by character, starting from index 0.
2. **Tracking '1's**:
    
    - Every time a '1' is encountered, it means that this '1' would need to be shifted later when a '0' is found, because the '0' needs to move to the left of all encountered '1's.
    - Therefore, for every '1', the `blackCount` is incremented by 1.
3. **Counting steps for '0's**:
    
    - When a '0' is encountered, it means this '0' would need to "pass" all the '1's that came before it in order to move to its correct position on the left.
    - The number of steps required to move this '0' is equal to the number of '1's it needs to pass, which is stored in `blackCount`.
    - `blackCount` is added to `res` for each '0'.
4. **Final result**:
    
    - After the entire string is processed, the total number of steps, `res`, is returned.

*TC ->* O( n )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    long long minimumSteps(string s) {
        long long res = 0;
        int blackCount = 0;

        for (char ch : s) {
            if (ch == '1')
                blackCount++;
            else
                res += blackCount;
        }

        return res;
    }
};
```
##### Two Pointers Approach
###### Key Variables:

1. **res (result)**: This will store the total number of steps required.
2. **low**: This variable tracks the position where the next '0' should go (it's initialized to -1 to handle the first '0').
3. **high**: This is the index that traverses the string `s` from left to right, checking each character.

###### Approach:

1. **Iterating through the string**:
    
    - The solution loops over the entire string using the `high` pointer, which checks every character (`ch`) in `s`.
2. **Finding '0's**:
    
    - Whenever a '0' is encountered (`if (ch == '0')`), the algorithm needs to "move" this '0' to the left towards its correct position.
    - `low` is incremented to indicate the next position where the '0' should be placed. Initially, `low = -1`, so the first '0' will go to index 0, the second '0' will go to index 1, and so on.
3. **Calculating the steps**:
    
    - For each '0' at index `high`, the distance it needs to move is calculated as `high - low`. This gives the number of steps needed to move this '0' to its correct position.
    - The distance is added to `res`, which keeps track of the total steps.
4. **Final Result**:
    
    - The total number of steps, `res`, is returned at the end.

*TC ->* O( n )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    long long minimumSteps(string s) {
        long long res = 0;
        int low = -1;

        for (int high = 0; high < s.size(); high++) {
            char ch = s[high];

            if (ch == '0') {
                low++;
                res += high - low;
            }
        }
        
        return res;
    }
};  
```