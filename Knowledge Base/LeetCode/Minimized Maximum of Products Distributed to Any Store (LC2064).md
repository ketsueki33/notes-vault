---
difficulty: medium
leetcode-num: 2064
topics:
  - Array
  - Binary Search
---
[Problem Link](https://leetcode.com/problems/minimized-maximum-of-products-distributed-to-any-store/)

#### Problem
You are given an integer `n` indicating there are `n` specialty retail stores. There are `m` product types of varying amounts, which are given as a **0-indexed** integer array `quantities`, where `quantities[i]` represents the number of products of the `ith` product type.

You need to distribute **all products** to the retail stores following these rules:

- A store can only be given **at most one product type** but can be given **any** amount of it.
- After distribution, each store will have been given some number of products (possibly `0`). Let `x` represent the maximum number of products given to any store. You want `x` to be as small as possible, i.e., you want to **minimize** the **maximum** number of products that are given to any store.

Return _the minimum possible_ `x`.

**Example 1:**

**Input:** n = 6, quantities = [11,6]
**Output:** 3
**Explanation:** One optimal way is:
- The 11 products of type 0 are distributed to the first four stores in these amounts: 2, 3, 3, 3
- The 6 products of type 1 are distributed to the other two stores in these amounts: 3, 3
The maximum number of products given to any store is max(2, 3, 3, 3, 3, 3) = 3.

**Example 2:**

**Input:** n = 7, quantities = [15,10,10]
**Output:** 5
**Explanation:** One optimal way is:
- The 15 products of type 0 are distributed to the first three stores in these amounts: 5, 5, 5
- The 10 products of type 1 are distributed to the next two stores in these amounts: 5, 5
- The 10 products of type 2 are distributed to the last two stores in these amounts: 5, 5
The maximum number of products given to any store is max(5, 5, 5, 5, 5, 5, 5) = 5.

**Example 3:**

**Input:** n = 1, quantities = [100000]
**Output:** 100000
**Explanation:** The only optimal way is:
- The 100000 products of type 0 are distributed to the only store.
The maximum number of products given to any store is max(100000) = 100000.

**Constraints:**

- `m == quantities.length`
- `1 <= m <= n <= 105`
- `1 <= quantities[i] <= 105`

#### Solution
[Video Explanation](https://youtu.be/VaUhSxN1-S0)


> [!info] Binary Search on Answer Pattern
> This is of the Binary Search on Answer Pattern because it is common for minimize the maximum type of problems.

##### Optimal Approach
###### 1. Helper Function `isPossible()`

The purpose of `isPossible(quantities, size, n)` is to check if it's possible to distribute items such that:

- No store gets more than `size` items.
- The total number of stores needed is `≤ n`.

***Details of `isPossible()`***

- **Loop through `quantities`**: For each quantity `x`, calculate the minimum number of stores required to accommodate `x` items without exceeding `size` items per store.
    - `ceil(double(x) / size)` gives the minimum number of stores required for `x` items, where each store holds at most `size` items.
- **Count stores**: Accumulate the total count of stores needed for all quantities. If at any point `count` exceeds `n`, we return `false` (since we can't distribute within the given `n` stores).
- **Return true if possible**: If the total store count is within `n`, the distribution is possible for this `size`.

###### 2. Main Function `minimizedMaximum()`

The goal of `minimizedMaximum()` is to find the smallest maximum value of items per store that allows a feasible distribution.

***Binary Search for Optimal Max Size***

- **Initialize bounds**:
    - `low` is set to 1 (smallest possible max size per store).
    - `high` is set to the maximum quantity in `quantities` (largest possible max size, where all items of one type go to one store).
- **Binary Search**:
    - **Mid Calculation**: Calculate the middle point `mid` of `low` and `high`—this is our current guess for the maximum number of items per store.
    - **Check Feasibility**:
        - If `isPossible(quantities, mid, n)` returns `true`, it means `mid` is a feasible solution, so update `res` to `mid` and try to find a smaller feasible max by moving `high` to `mid - 1`.
        - If `isPossible(quantities, mid, n)` is `false`, it means `mid` is too small, so we increase `low` to `mid + 1`.
- **Result**: The smallest feasible maximum is stored in `res` and returned as the answer.


*TC ->* O( `m * log(maxVal)` ), where m is size of quantities
*SC ->* O( 1 )

```cpp title=Code
class Solution {
    bool isPossible(vector<int>& quantities, int size, int n) {
        int count = 0;

        for (int x : quantities) {
            count += ceil(double(x) / size);

            if (count > n)
                return false;
        }

        return true;
    }

public:
    int minimizedMaximum(int n, vector<int>& quantities) {
        int low = 1;
        int high = *max_element(quantities.begin(), quantities.end());
        int res;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (isPossible(quantities, mid, n)) {
                res = mid;
                high = mid - 1;
            } else
                low = mid + 1;
        }

        return res;
    }
};
```