---
difficulty: easy
leetcode-num: 1331
topics:
  - Array
  - Hash Table
  - Sorting
---
[Problem Link](https://leetcode.com/problems/rank-transform-of-an-array/)

#### Problem
Given an array of integers `arr`, replace each element with its rank.

The rank represents how large the element is. The rank has the following rules:

- Rank is an integer starting from 1.
- The larger the element, the larger the rank. If two elements are equal, their rank must be the same.
- Rank should be as small as possible.

**Example 1:**

**Input:** arr = `[40,10,20,30]`
**Output:** `[4,1,2,3]`
**Explanation**: 40 is the largest element. 10 is the smallest. 20 is the second smallest. 30 is the third smallest.

**Example 2:**

**Input:**`arr = [100,100,100]`
**Output:** `[1,1,1]`
**Explanation**: Same elements share the same rank.

**Example 3:**

**Input:**` arr = [37,12,28,9,100,56,80,5,12]`
**Output:** `[5,3,4,2,8,6,7,1,3]`

**Constraints:**

- `0 <= arr.length <= 105`
- `-109 <= arr[i] <= 109`

#### Solution
##### Optimal Approach

1. **Copy the original array**: A copy of the input array `arr` is made and stored in `v`, allowing us to sort `v` while leaving `arr` unchanged for now.
    
2. **Sort the array**: The copy `v` is sorted in ascending order. This allows us to rank elements in order of their size.
    
3. **Rank the elements using a map**:
    
    - We iterate over the sorted array `v`.
    - For each element `x`, we attempt to insert it into the unordered map `mp` using the `emplace()` function. The value stored in the map corresponds to the rank of the element, which is calculated as the current size of the map plus 1.
    - `emplace()` ensures that if an element has already been inserted into the map (i.e., it's a duplicate), it will not overwrite the existing rank. This ensures that each unique element gets a rank exactly once.
4. **Update the original array with ranks**: After building the rank map, we iterate over the original array `arr`. For each element in `arr`, we look up its corresponding rank in the map `mp` and replace the element with its rank.
    
5. **Return the ranked array**: Finally, the transformed `arr` is returned, where each element is replaced by its rank.

###### Why `emplace()` Works:

The use of `emplace()` is critical in this solution because it inserts the element into the map only if it doesn't already exist. This is important for handling duplicate elements:

- If `mp.emplace(x, rank)` is called and `x` already exists in the map, the insertion is skipped, and the original rank is preserved.
- This prevents the map from storing duplicate keys, meaning that all occurrences of a particular element in `arr` will share the same rank.

In contrast, using `mp[x] = rank` would overwrite the rank if a duplicate is encountered, leading to incorrect results. By using `emplace()`, the solution ensures that each element gets a unique rank without duplicates being inserted multiple times.

*TC ->* O( `n * log n` )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    vector<int> arrayRankTransform(vector<int>& arr) {
        unordered_map<int, int> mp;
        vector<int> v(arr.begin(), arr.end());

        sort(v.begin(), v.end());

        for (int x : v) {
            mp.emplace(x, mp.size() + 1);
        }

        for (int i = 0; i < arr.size(); i++) {
            arr[i] = mp[arr[i]];
        }

        return arr;
    }
};
```