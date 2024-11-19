---
difficulty: medium
leetcode-num: 264
topics:
  - Hash Table
  - Math
  - Dynamic Programming
  - Heap
---
[Problem Link](https://leetcode.com/problems/ugly-number-ii/)

#### Problem
An **ugly number** is a positive integer whose prime factors are limited to `2`, `3`, and `5`.

Given an integer `n`, return _the_ `nth` _**ugly number**_.

**Example 1:**

**Input:** n = 10
**Output:** 12
**Explanation:** `[1, 2, 3, 4, 5, 6, 8, 9, 10, 12]` is the sequence of the first 10 ugly numbers.

**Example 2:**

**Input:** n = 1
**Output:** 1
**Explanation:** 1 has no prime factors, therefore all of its prime factors are limited to 2, 3, and 5.

**Constraints:**

- `1 <= n <= 1690`

#### Solution
[Video Explanation](https://youtu.be/1pj2a5bmziY)

##### DP Approach

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> nums = {1};
        int i2 = 0, i3 = 0, i5 = 0;

        for (int i = 0; i < n - 1 ; i++) {
            int next = min(nums[i2] * 2, min(nums[i3] * 3, nums[i5] * 5));

            nums.push_back(next);

            if (next == nums[i2] * 2)
                i2++;
            if (next == nums[i3] * 3)
                i3++;
            if (next == nums[i5] * 5)
                i5++;
        }
        return nums.back();
    }
};
```

##### Min Heap & Hash Set Approach

*TC ->* O( `n * log n` )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    long nthUglyNumber(int n) {
        priority_queue<long, vector<long>, greater<long>> pq;
        unordered_set<long> s;
        pq.push(1);

        while (n > 1) {
            long x = pq.top();
            pq.pop();
            long x2 = x * 2;
            long x3 = x * 3;
            long x5 = x * 5;

            if (!s.count(x2)) {
                s.insert(x2);
                pq.push(x2);
            }
            if (!s.count(x3)) {
                s.insert(x3);
                pq.push(x3);
            }
            if (!s.count(x5)) {
                s.insert(x5);
                pq.push(x5);
            }
            n--;
        }

        return pq.top();
    }
};
```