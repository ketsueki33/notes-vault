---
difficulty: medium
leetcode-num: 503
topics:
  - Array
  - Stack
  - Monotonic Stack
---
[Problem Link](https://leetcode.com/problems/next-greater-element-ii/)

#### Problem
Given a circular integer array `nums` (i.e., the next element of `nums[nums.length - 1]` is `nums[0]`), return _the **next greater number** for every element in_ `nums`.

The **next greater number** of a number `x` is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, return `-1` for this number.

**Example 1:**

**Input:** nums = [1,2,1]
**Output:** [2,-1,2]
Explanation: The first 1's next greater number is 2; 
The number 2 can't find next greater number. 
The second 1's next greater number needs to search circularly, which is also 2.

**Example 2:**

**Input:** nums = [1,2,3,4,3]
**Output:** [2,3,4,-1,4]

**Constraints:**

- `1 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
#### Solution
##### Optimal Approach
- **Circular Array Simulation**: Since the array is circular, the loop runs twice the size of the array (`2 * n - 1`), meaning we effectively traverse the array twice. The modulo operation (`i % n`) helps us wrap around when we reach the end of the array, simulating the circular behavior.
    
- **Monotonic Stack**: A stack is used to store potential candidates for the next greater element in decreasing order. As we traverse the array from the back, we maintain this order:
    
    - If the current number (`num`) is greater than or equal to the top of the stack, we pop elements from the stack.
    - If the stack isn’t empty after this, the top of the stack represents the next greater element for the current number.
- **Updating Result**: For each element, after processing the stack, if the stack isn’t empty, the top of the stack gives the next greater element for the current number.
    
- **Edge Case**: If no greater element is found (the stack is empty), the default value in the result remains `-1`.


*TC ->* O( n )
*SC ->* O(n  )

```cpp title=Code
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        vector<int> res( nums.size() , -1 );
        stack<int> st;
        int n = nums.size();
 
        for(int i = 2*n - 1 ; i >= 0 ; i--   ){
            int idx = i%n;
            int num = nums[idx];

            while (!st.empty() && num >= st.top())
                st.pop();
            
            if ( !st.empty()) {
                res[idx] = st.top();
            }

            st.push(num);
        }
        
        return res;
    }
};
```


#### Similar Problems
- [[Next Greater Element I (LC496)]]