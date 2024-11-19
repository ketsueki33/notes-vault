---
difficulty: medium
leetcode-num: 2070
topics:
  - Array
  - Binary Search
  - Sorting
---
[Problem Link](https://leetcode.com/problems/most-beautiful-item-for-each-query/)

#### Problem
You are given a 2D integer array `items` where `items[i] = [pricei, beautyi]` denotes the **price** and **beauty** of an item respectively.

You are also given a **0-indexed** integer array `queries`. For each `queries[j]`, you want to determine the **maximum beauty** of an item whose **price** is **less than or equal** to `queries[j]`. If no such item exists, then the answer to this query is `0`.

Return _an array_ `answer` _of the same length as_ `queries` _where_ `answer[j]` _is the answer to the_ `jth` _query_.

**Example 1:**

**Input:** `items = [[1,2],[3,2],[2,4],[5,6],[3,5]], queries = [1,2,3,4,5,6]`
**Output:** [2,4,5,5,6,6]
**Explanation:**
- For queries[0]=1, [1,2] is the only item which has price <= 1. Hence, the answer for this query is 2.
- For queries[1]=2, the items which can be considered are [1,2] and [2,4]. 
  The maximum beauty among them is 4.
- For queries[2]=3 and queries[3]=4, the items which can be considered are [1,2], [3,2], [2,4], and [3,5].
  The maximum beauty among them is 5.
- For queries[4]=5 and queries[5]=6, all items can be considered.
  Hence, the answer for them is the maximum beauty of all items, i.e., 6.

**Example 2:**

**Input:** `items = [[1,2],[1,2],[1,3],[1,4]], queries = [1]`
**Output:** [4]
**Explanation:** 
The price of every item is equal to 1, so we choose the item with the maximum beauty 4. 
Note that multiple items can have the same price and/or beauty.  

**Example 3:**

**Input:** `items = [[10,1000]], queries = [5]`
**Output:** [0]
**Explanation:**
No item has a price less than or equal to 5, so no item can be chosen.
Hence, the answer to the query is 0.

**Constraints:**

- `1 <= items.length, queries.length <= 105`
- `items[i].length == 2`
- `1 <= pricei, beautyi, queries[j] <= 109`

#### Solution
[Video Explanation](https://youtu.be/kZGRjC7p3AE)

##### Optimal Approach
- **Sorting the Items**:
    
    - `items` is first sorted by price, so we can efficiently perform binary search on prices.
    - After sorting, we can treat prices as increasing, which helps when finding the highest beauty within a budget.
- **Updating Beauty in `items`**:
    
    - We modify `items` to store the maximum beauty obtainable up to each price index.
    - We loop through `items`, and for each item, update its beauty value to the maximum beauty seen so far. This way, each entry at `items[i]` will contain the maximum beauty available at prices up to `items[i][0]`.
    - This preprocessing step allows us to easily look up the maximum beauty within a given budget.
- **Binary Search with `getItem`**:
    
    - The `getItem` function performs a binary search on `items` to find the most expensive item that doesn’t exceed the budget.
    - During the binary search:
        - If `items[mid][0]` (price of the mid item) is within the budget, update `beauty` to `items[mid][1]` (current max beauty for this price) and move `low` to `mid + 1` to check if a higher-priced item can still fit within the budget.
        - If `items[mid][0]` exceeds the budget, adjust `high` to `mid - 1` to check lower-priced items.
    - The result is the highest beauty obtainable without exceeding the budget.
- **Answering Queries**:
    
    - For each budget in `queries`, we use `getItem` to find the maximum beauty that can be bought within that budget.
    - Results are collected in `res` and returned.

*TC ->* O( `n log n + m log n` ) , where n is the number of items & m is number of queries
*SC ->* O( 1 )

```cpp title=Code
class Solution {
    int getItem(vector<vector<int>>& items, int budget) {
        int low = 0;
        int high = items.size() - 1;
        int beauty = 0;

        while (low <= high) {
            int mid = low + (high - low) / 2;
            int price = items[mid][0];

            if (price <= budget) {
                beauty = items[mid][1];
                low = mid + 1;
            } else
                high = mid - 1;
        }

        return beauty;
    }

public:
    vector<int> maximumBeauty(vector<vector<int>>& items,
                              vector<int>& queries) {
        vector<int> res;

        sort(items.begin(), items.end());

        int maxBeautySoFar = -1;

        // update items to hold max beauty upto that index
        for (auto& x : items) {
            maxBeautySoFar = max(maxBeautySoFar, x[1]);
            x[1] = maxBeautySoFar;
        }

        for (int x : queries)
            res.push_back(getItem(items, x));

        return res;
    }
};
```