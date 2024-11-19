---
difficulty: medium
leetcode-num: 2601
topics:
  - Array
  - Math
  - Binary Search
  - Greedy
  - Number Theory
---
[Problem Link](https://leetcode.com/problems/prime-subtraction-operation/)

#### Problem
You are given a **0-indexed** integer array `nums` of length `n`.

You can perform the following operation as many times as you want:

- Pick an index `i` that you haven’t picked before, and pick a prime `p` **strictly less than** `nums[i]`, then subtract `p` from `nums[i]`.

Return _true if you can make `nums` a strictly increasing array using the above operation and false otherwise._

A **strictly increasing array** is an array whose each element is strictly greater than its preceding element.

**Example 1:**

**Input:** nums = [4,9,6,10]
**Output:** true
**Explanation:** In the first operation: Pick i = 0 and p = 3, and then subtract 3 from nums[0], so that nums becomes [1,9,6,10].
In the second operation: i = 1, p = 7, subtract 7 from nums[1], so nums becomes equal to [1,2,6,10].
After the second operation, nums is sorted in strictly increasing order, so the answer is true.

**Example 2:**

**Input:** nums = [6,8,11,12]
**Output:** true
**Explanation:** Initially nums is sorted in strictly increasing order, so we don't need to make any operations.

**Example 3:**

**Input:** nums = [5,8,3]
**Output:** false
**Explanation:** It can be proven that there is no way to perform operations to make nums sorted in strictly increasing order, so the answer is false.

**Constraints:**

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`
- `nums.length == n`

#### Solution

##### Optimal Approach


1. **Prime Number Generation with Sieve of Eratosthenes**:
    
    - The `findPrime` function generates the first 1000 prime numbers using the Sieve of Eratosthenes. It initializes a `vector<bool>` array called `prime`, where each index represents if the number at that index is prime.
    - For each integer `p` from 2 up to the square root of `n`, if `p` is still marked as `true` (prime), it marks all multiples of `p` as `false` (not prime).
    - Finally, for each number `p` that remains `true`, the function adds `p` to the `primes` set. The set stores all prime numbers up to 1000 in sorted order for fast lookups.
2. **Getting the Largest Prime Less than a Target (`getPrime` function)**:
    
    - The function `getPrime` finds the largest prime number in `primes` that is less than a given `target`.
    - It uses `lower_bound` to find the smallest prime greater than or equal to `target`. If the result points to the first element and it’s larger than the target, it returns `0` to indicate that no prime is less than `target`.
    - Otherwise, it decrements the iterator to get the largest prime strictly less than `target` and returns that prime.
3. **The Main Function - `primeSubOperation`**:
    
    - The main function `primeSubOperation` checks if each element in the input array `nums` can be adjusted (by subtracting a prime number) to satisfy a strictly increasing sequence.
    - For each element `x` in `nums`, it:
        - Calculates `diff`, which is the difference between `x` and the previous value `prev`.
        - Finds the largest prime that is less than `diff` using `getPrime(diff)`, and subtracts this prime from `x` to get `next`.
    - If the `next` value is not greater than `prev`, the function returns `false` (the sequence cannot be strictly increasing with the given adjustments).
    - If all elements pass the check, the function returns `true`.

*TC ->* O( `n * log(k) + n log(log(1000))` )
- where k is the number of primes less than 1000

*SC ->* O( 1000 ) , for `prime` boolean array in `findPrime`

```cpp title=Code
class Solution {
    set<int> primes;


    // Sieve of Erastothenes to find first 1000 primes
    void findPrime(int n) {
        vector<bool> prime(n + 1, true);

        for (int p = 2; p * p <= n; p++) {

            if (prime[p] == true) {
                for (int i = p * p; i <= n; i += p)
                    prime[i] = false;
            }
        }

        for (int p = 2; p <= n; p++)
            if (prime[p])
                primes.insert(p);
    }

    int getPrime(int target) {
        auto it = primes.lower_bound(target);
        if (it == primes.begin()) {
            return 0;
        }
        --it;
        return *it;
    }

public:
    bool primeSubOperation(vector<int>& nums) {
        findPrime(1000);

        int prev = 0;
        for (int x : nums) {
            int diff = x - prev;
            int next = x - getPrime(diff);
            
            if( prev >= next )
                return false;
            
            prev = next;
        }

        return true;
    }
};
```