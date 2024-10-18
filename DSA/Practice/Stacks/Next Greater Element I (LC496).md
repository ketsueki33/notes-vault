---
difficulty: easy
leetcode-num: 496
topics:
  - Array
  - Hash Table
  - Stack
  - Monotonic Stack
---
[Problem Link](https://leetcode.com/problems/next-greater-element-i/)

#### Problem
The **next greater element** of some element `x` in an array is the **first greater** element that is **to the right** of `x` in the same array.

You are given two **distinct 0-indexed** integer arrays `nums1` and `nums2`, where `nums1` is a subset of `nums2`.

For each `0 <= i < nums1.length`, find the index `j` such that `nums1[i] == nums2[j]` and determine the **next greater element** of `nums2[j]` in `nums2`. If there is no next greater element, then the answer for this query is `-1`.

Return _an array_ `ans` _of length_ `nums1.length` _such that_ `ans[i]` _is the **next greater element** as described above._

**Example 1:**

**Input:** nums1 = [4,1,2], nums2 = [1,3,4,2]
**Output:** [-1,3,-1]
**Explanation:** The next greater element for each value of nums1 is as follows:
- 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
- 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.
- 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.

**Example 2:**

**Input:** nums1 = [2,4], nums2 = [1,2,3,4]
**Output:** [3,-1]
**Explanation:** The next greater element for each value of nums1 is as follows:
- 2 is underlined in nums2 = [1,2,3,4]. The next greater element is 3.
- 4 is underlined in nums2 = [1,2,3,4]. There is no next greater element, so the answer is -1.

**Constraints:**

- `1 <= nums1.length <= nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 104`
- All integers in `nums1` and `nums2` are **unique**.
- All the integers of `nums1` also appear in `nums2`.

#### Solution
##### Optimal Approach (Monotonic Stack)


> [!info] 
> For better understanding of Monotonic stacks, see [[../../Theory/Stacks#Monotonic Stack|Monotonic Stacks]]

---

1. **Mapping Positions:**
    
    - First, the code creates a hash map (`mp`) that stores the index of each element in `nums1`. This helps to quickly find the corresponding index of each element of `nums1`  in `res`.
2. **Monotonic Stack for NGE:**
    
    - The idea is to use a stack to maintain the elements of `nums2` in a **monotonic decreasing order** while scanning from right to left. This allows the solution to track the next greater element efficiently.
    - If an element is smaller than the current element being processed, it stays in the stack because it could still be the NGE for future elements. If it's greater, it gets popped from the stack, as it can't be the NGE for future elements.
3. **Iterating over `nums2`:**
    
    - The code iterates over `nums2` in reverse order (right to left).
    - For each element `num` in `nums2`:
        - It pops all elements from the stack that are smaller or equal to `num`, ensuring the stack always maintains the next greater elements.
        - If the current element `num` is in `nums1` (checked using the map `mp`), it assigns the top of the stack as the NGE for that element (if the stack is not empty). If the stack is empty, the NGE is `-1` (default in `res`).
4. **Final Result:**
    
    - After the loop, `res` contains the next greater elements for all the elements in `nums1`, based on their corresponding positions in `nums2`.

*TC ->* O( n + m )
*SC ->* O( m )

```cpp title=Code
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> mp;
        vector<int> res(nums1.size(), -1);
        stack<int> st;

        for (int i = 0; i < nums1.size(); i++)
            mp[nums1[i]] = i;

        for (int i = nums2.size() - 1; i >= 0; i--) {
            int num = nums2[i];

            while (!st.empty() && num >= st.top())
                st.pop();

            if (mp.count(num) && !st.empty()) {
                res[mp[num]] = st.top();
            }

            st.push(num);
        }
        return res;
    }
};
```

##### Brute Force
We use nested loops to check each element in the array. For every element in the first array (`nums1`), we look for its position in the second array (`nums2`), then iterate over the remaining elements in `nums2` (after the current element) to find the first element that is greater. If no greater element is found, we assign `-1` as the NGE. This approach has a time complexity of O(n * m), where `n` is the size of `nums1` and `m` is the size of `nums2`.

*TC ->* O( `n * m` )
*SC ->* O( 1 )

#### Similar Problems
- [[Next Greater Element II (LC503)]]