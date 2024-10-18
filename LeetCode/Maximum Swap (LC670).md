---
difficulty: medium
leetcode-num: 670
topics:
  - Math
  - Greedy
---
[Problem Link](https://leetcode.com/problems/maximum-swap/)

#### Problem
You are given an integer `num`. You can swap two digits at most once to get the maximum valued number.

Return _the maximum valued number you can get_.

**Example 1:**

**Input:** num = 2736
**Output:** 7236
**Explanation:** Swap the number 2 and the number 7.

**Example 2:**

**Input:** num = 9973
**Output:** 9973
**Explanation:** No swap.

**Constraints:**

- `0 <= num <= 108`

#### Solution
##### Optimal Approach
###### Key Variables:

- **`numStr`**: Converts the given number into a string to manipulate individual digits.
- **`greatest` and `greatestIdx`**: Tracks the greatest digit encountered so far (from the right side of the number) and its index. This helps in finding a larger digit later that can be swapped.
- **`left` and `right`**: These variables store the positions of the two digits to be swapped (the digit on the left is smaller than the greatest digit on the right).

###### Step-by-step Explanation:

1. **Convert Number to String**: The number is converted to a string, making it easier to work with individual digits.
    
2. **Initialize Tracking Variables**: The last digit is set as the initial **greatest** digit, and its index is stored in `greatestIdx`.
    
3. **Traverse the Digits from Right to Left**: The loop runs from the second last digit (`n-2`) to the first digit (`0`).
    
    - **Comparison**: At each digit `i`, compare it with the current **greatest** digit found on its right:
        - If the **greatest** digit is larger than the current digit `i`, then it's a candidate for swapping to increase the number. So, `left` is updated to `i` and `right` to `greatestIdx`.
        - If the current digit `i` is larger than the greatest digit found so far, then update the `greatest` and `greatestIdx` variables. This ensures that you're always tracking the largest possible digit to swap.
4. **Swap**: If a swap has been found (i.e., `left` is not -1), swap the digits at the **left** and **right** indices.
    
    If no swap is found (`left` remains -1), it means the number is already the largest possible arrangement, so return the original number.
    
5. **Return the Result**: After swapping, convert the string back to an integer using `stoi()` and return the modified number.


*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    int maximumSwap(int num) {
        string numStr = to_string(num);
        int n = numStr.size();

        char greatest = numStr[n - 1];
        int greatestIdx = n - 1;

        int left = -1, right = -1;

        for (int i = n - 2; i >= 0; i--) {
            if (greatest > numStr[i]) {
                left = i;
                right = greatestIdx;
            }

            if (greatest < numStr[i]) {
                greatest = numStr[i];
                greatestIdx = i;
            }
        }

        if (left == -1)
            return num;

        swap(numStr[left], numStr[right]);
        return stoi(numStr);
    }
};
```