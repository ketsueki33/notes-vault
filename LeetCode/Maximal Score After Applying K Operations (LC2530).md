---
difficulty: medium
leetcode-num: 2530
topics:
  - Array
  - Greedy
  - Heap
---
[Problem Link]()

#### Problem
You are given a **0-indexed** integer array `nums` and an integer `k`. You have a **starting score** of `0`.

In one **operation**:

1. choose an index `i` such that `0 <= i < nums.length`,
2. increase your **score** by `nums[i]`, and
3. replace `nums[i]` with `ceil(nums[i] / 3)`.

Return _the maximum possible **score** you can attain after applying **exactly**_ `k` _operations_.

The ceiling function `ceil(val)` is the least integer greater than or equal to `val`.

**Example 1:**

**Input:** `nums = [10,10,10,10,10], k = 5`
**Output:** 50
**Explanation:** Apply the operation to each array element exactly once. The final score is 10 + 10 + 10 + 10 + 10 = 50.

**Example 2:**

**Input:** nums = `[1,10,3,3,3], k = 3`
**Output:** 17
**Explanation:** You can do the following operations:
Operation 1: Select i = 1, so nums becomes [1,**4**,3,3,3]. Your score increases by 10.
Operation 2: Select i = 1, so nums becomes [1,**2**,3,3,3]. Your score increases by 4.
Operation 3: Select i = 2, so nums becomes [1,1,**1**,3,3]. Your score increases by 3.
The final score is 10 + 4 + 3 = 17.

**Constraints:**

- `1 <= nums.length, k <= 105`
- `1 <= nums[i] <= 109`

#### Solution
##### Optimal Approach
- **Initial Setup**:
    
    - We start by using a priority queue (max-heap) to store all the elements from the input array. This will ensure that we can always extract the largest element efficiently.
- **Main Loop**:
    
    - We repeatedly perform operations for `k` times:
        - In each iteration, the largest element is extracted from the heap.
        - This element is added to the total result.
        - Then, the largest element is divided by 3, and the ceiling of this value is pushed back into the heap. This ensures that the next time we retrieve a large number again, it will either be the modified value or some other large number that was already in the heap.
- **Final Sum**:
    
    - After performing `k` operations, the accumulated sum gives the maximum possible result based on the strategy of selecting and reducing the largest element.


> [!NOTE] Why `num / 3.0` Instead of `num / 3`?
> We use `num / 3.0` to ensure floating-point division, which allows us to get the ceiling of the result after division. If we used `num / 3` (integer division), it would truncate the result, potentially missing a higher value that comes from rounding up, which is crucial for maximizing the result.


*TC ->* O( `n + k * log n ` )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    long long maxKelements(vector<int>& nums, int k) {
        priority_queue<int> pq(nums.begin(), nums.end());

        long long res = 0;

        while (k--) {
            int num = pq.top();
            pq.pop();

            res += num;
            pq.push(ceil(num / 3.0));
        }
        return res;
    }
};
```