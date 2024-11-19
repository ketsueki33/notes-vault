---
difficulty: medium
leetcode-num: 1310
topics:
  - Bit Manipulation
  - Array
  - Prefix Sum
---
[Problem Link](https://leetcode.com/problems/xor-queries-of-a-subarray/)

#### Problem

You are given an array `arr` of positive integers. You are also given the array `queries` where `queries[i] = [lefti, righti]`.

For each query `i` compute the **XOR** of elements from `lefti` to `righti` (that is, `arr[lefti] XOR arr[lefti + 1] XOR ... XOR arr[righti]` ).

Return an array `answer` where `answer[i]` is the answer to the `ith` query.

**Example 1:**

**Input:** `arr = [1,3,4,8], queries = [[0,1],[1,2],[0,3],[3,3]]`
**Output:** `[2,7,14,8] `
**Explanation:** 
The binary representation of the elements in the array are:
1 = 0001 
3 = 0011 
4 = 0100 
8 = 1000 
The XOR values for queries are:
`[0,1]` = 1 xor 3 = 2 
`[1,2] `= 3 xor 4 = 7 
`[0,3]` = 1 xor 3 xor 4 xor 8 = 14 
`[3,3]` = 8

**Example 2:**

**Input:** `arr = [4,8,2,10], queries = [[2,3],[1,3],[0,0],[0,3]]`
**Output:** `[8,0,4,4]`

**Constraints:**

- `1 <= arr.length, queries.length <= 3 * 104`
- `1 <= arr[i] <= 109`
- `queries[i].length == 2`
- `0 <= lefti <= righti < arr.length`
#### Solution
[Video Explanation](https://youtu.be/rZYdNpHXz0o)

We well keep the cumulative XOR stored in a `preXOR` array so that `preXOR[i]` is the XOR of all elements from `0` to `i` index.

Since , XOR of a number with itself is `0`, we can use the `preXOR` array to get instant result.

![[../../_assets/XOR Queries of a Subarrray.png|500]]

So we will use do `preXOR[right] ^ preXOR[left - 1]` for every query except when `left` is `0`.

*TC ->* O( n + q ), where q is the number of queries
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    vector<int> xorQueries(vector<int>& arr, vector<vector<int>>& queries) {
        vector<int> preXOR(arr.size());
        vector<int> res;

        preXOR[0] = arr[0];

        for (int i = 1; i < arr.size(); i++) {
            preXOR[i] = (preXOR[i - 1] ^ arr[i]);
        }

        for (auto& q : queries) {
            int left = q[0];
            int right = q[1];
            if (left > 0)
                res.push_back(preXOR[right] ^ preXOR[left - 1]);
            else
                res.push_back(preXOR[right]);
        }
        return res;
    }
};
```