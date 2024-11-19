---
difficulty: medium
leetcode-num: 1497
topics:
  - Array
  - Hash Table
  - Counting
---
[Problem Link](https://leetcode.com/problems/check-if-array-pairs-are-divisible-by-k/)

#### Problem
Given an array of integers `arr` of even length `n` and an integer `k`.

We want to divide the array into exactly `n / 2` pairs such that the sum of each pair is divisible by `k`.

Return `true` _If you can find a way to do that or_ `false` _otherwise_.

**Example 1:**

**Input:**` arr = [1,2,3,4,5,10,6,7,8,9], k = 5`
**Output:** true
**Explanation:** Pairs are (1,9),(2,8),(3,7),(4,6) and (5,10).

**Example 2:**

**Input:** `arr = [1,2,3,4,5,6], k = 7`
**Output:** true
**Explanation:** Pairs are (1,6),(2,5) and(3,4).

**Example 3:**

**Input:** `arr = [1,2,3,4,5,6], k = 10`
**Output:** false
**Explanation:** You can try all possible pairs to see that there is no way to divide arr into 3 pairs each with sum divisible by 10.

**Constraints:**

- `arr.length == n`
- `1 <= n <= 105`
- `n` is even.
- `-109 <= arr[i] <= 109`
- `1 <= k <= 105`

#### Solution
[Video Explanation](https://youtu.be/Lye_llDcSuI)

##### Optimal Approach

> [!Important] 
> The sum of two numbers is divisible by `k` if and only if the sum of their remainders is equal to `k` or both remainders are `0`.

- **Remainders and Modulo Arithmetic**:
    
    - For any two numbers, if their sum is divisible by `k`, then their remainders when divided by `k` must sum up to `k`. ( unless the both numbers are perfectly divisible)
    - For example, if `x % k = 2` and `y % k = 3`, then `x + y % k = (2 + 3) % k = 0` (assuming `k = 5`). Hence, `x + y` is divisible by `k`.
    - This means that the remainder of one number must complement the remainder of another number, i.e., `remainder1 + remainder2 = k`.
- **Tracking Remainders**:
    
    - We use a vector `mp` of size `k` to count the occurrences of each remainder when the elements of the array are divided by `k`. The index of this array corresponds to the remainder value.
    - The formula `(x % k + k) % k` ensures that even negative numbers yield valid remainders between `0` and `k-1`.
- **Step-by-step process**:
    
    - **Step 1**: Loop through each element in the array `arr` and calculate its remainder when divided by `k`. This remainder is stored in `mp` at the corresponding index.
    - **Step 2**: Special case for remainder `0`. If a number is perfectly divisible by `k`, its remainder is `0`. These numbers need to pair with each other, so the count of numbers with remainder `0` must be even. If it's not, we return `false`.
    - **Step 3**: For each remainder `i` from `1` to `k / 2` (inclusive), we check if there are equal counts of elements with remainder `i` and `k - i` (because these two remainders must pair together to be divisible by `k`).
        - **Special case**: When `i == k - i` (i.e., when `k` is even, and we're checking the middle remainder like `k = 6` and `i = 3`), the count of elements with that remainder must be even, as they need to pair with themselves.
- **Final Check**:
    
    - After checking all the remainders and their complements, if all conditions are satisfied, the function returns `true`, meaning the array can be rearranged into valid pairs. Otherwise, it returns `false`.

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    bool canArrange(vector<int>& arr, int k) {
        // array to store remainders ( remainder max value is k-1)
        vector<int> mp(k, 0);

        for (int x : arr) {
            // extra processing to handle negative values
            int idx = (x % k + k) % k;
            mp[idx]++;
        }

        if (mp[0] % 2 != 0)
            return false;

        for (int i = 1; i <= k / 2; i++) {
            if (i == k - i && mp[i] % 2 != 0)
                return false;
            if (mp[i] != mp[k - i])
                return false;
        }
        return true;
    }
};
```