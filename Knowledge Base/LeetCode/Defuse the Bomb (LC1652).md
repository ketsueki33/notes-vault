---
difficulty: easy
leetcode-num: 1652
topics:
  - Array
  - Sliding Window
---
[Problem Link](https://leetcode.com/problems/defuse-the-bomb/)

#### Problem

You have a bomb to defuse, and your time is running out! Your informer will provide you with a **circular** array `code` of length of `n` and a key `k`.

To decrypt the code, you must replace every number. All the numbers are replaced **simultaneously**.

- If `k > 0`, replace the `ith` number with the sum of the **next** `k` numbers.
- If `k < 0`, replace the `ith` number with the sum of the **previous** `k` numbers.
- If `k == 0`, replace the `ith` number with `0`.

As `code` is circular, the next element of `code[n-1]` is `code[0]`, and the previous element of `code[0]` is `code[n-1]`.

Given the **circular** array `code` and an integer key `k`, return _the decrypted code to defuse the bomb_!

**Example 1:**

**Input:** code = [5,7,1,4], k = 3
**Output:** [12,10,16,13]
**Explanation:** Each number is replaced by the sum of the next 3 numbers. The decrypted code is [7+1+4, 1+4+5, 4+5+7, 5+7+1]. Notice that the numbers wrap around.

**Example 2:**

**Input:** code = [1,2,3,4], k = 0
**Output:** [0,0,0,0]
**Explanation:** When k is zero, the numbers are replaced by 0. 

**Example 3:**

**Input:** code = [2,4,9,3], k = -2
**Output:** [12,5,6,13]
**Explanation:** The decrypted code is [3+9, 2+3, 4+2, 9+4]. Notice that the numbers wrap around again. If k is negative, the sum is of the **previous** numbers.

**Constraints:**

- `n == code.length`
- `1 <= n <= 100`
- `1 <= code[i] <= 100`
- `-(n - 1) <= k <= n - 1`
#### Solution

##### Optimal Approach 1
[Video Explanation](https://youtu.be/QjO6T1cThRU)

- **Edge Case: `k=0`**  
    If `k` is zero, the algorithm immediately returns an array of zeros since no elements need to be summed.
    
- **Set Initial Window Bounds**  
    The range of elements to sum depends on the value of `k`:
    
    - For `k > 0`, the initial window includes the next `k` elements, starting at index 1 and ending at index `k`.
    - For `k < 0`, the initial window includes the previous `|k|` elements, wrapping around to the end of the array if necessary. The start is `n - |k|`, and the end is `n - 1`.
- **Calculate Initial Window Sum**  
    The algorithm computes the sum of the elements within the initial window.
    
- **Slide the Window Across the Array**  
    For each element in the `code` array:
    
    - The current window sum is stored in the result array at the corresponding index.
    - The value at the "start" of the window is subtracted from the sum as it slides out of the window.
    - The value at the next position after the "end" of the window is added as it slides into the window.
    - Both the start and end of the window are updated using modular arithmetic to ensure circular indexing.
- **Return the Result**  
    Once all elements are processed, the `res` array containing the decrypted values is returned.

*TC ->* O( n )
*SC ->* O( n ), if counting `res`

```cpp title=Code
class Solution {

public:
    vector<int> decrypt(vector<int>& code, int k) {
        int n = code.size();
        vector<int> res(n, 0);

        if (k == 0)
            return res;

        int low, high;

        if (k > 0) {
            low = 1;
            high = k;
        } else {
            low = n - abs(k);
            high = n - 1;
        }

        int windowSum = 0;

        for (int i = low; i <= high; i++) {
            windowSum += code[i];
        }

        for (int i = 0; i < n; i++) {
            res[i] = windowSum;

            windowSum -= code[(low) % n];
            low++;

            windowSum += code[(high + 1) % n];
            high++;
        }

        return res;
    }
};
```

##### Optimal Approach 2

- **Handle Edge Cases**  
    If `k` is zero, the result is initialized as all zeros and returned immediately.
    
- **Manage Negative `k` Values**  
    If `k` is negative, the algorithm reverses the array and treats the task as summing the next `|k|` elements. This allows handling positive and negative `k` with a unified logic.
    
- **Sliding Window Setup**  
    A sliding window is used to compute the sum of the next `k` elements efficiently:
    
    - Two pointers, `low` and `high`, represent the bounds of the sliding window.
    - The window initially spans the first `k` elements. The sum of this window is calculated.
- **Sliding the Window Across the Array**  
    For each element:
    
    - The current window sum is stored in the result array.
    - The `low` pointer moves forward circularly, removing its value from the sum.
    - The `high` pointer also moves forward circularly, adding its value to the sum.
- **Restore Array for Negative `k`**  
    If the array was reversed for handling negative `k`, the result is reversed again to restore the correct order.

*TC ->* O( n )
*SC ->* O( n ), if counting `res`

```cpp title=Code
class Solution {
    int ni(int n, int i) { // next index
        return (i + 1) % n;
    }

public:
    vector<int> decrypt(vector<int>& code, int k) {
        int n = code.size();
        vector<int> res(n, 0);

        if (k == 0)
            return res;

        bool negative = false;
        if (k < 0) {
            negative = true;
            k *= -1;
            reverse(code.begin(), code.end());
        }

        int low = n - 1, sum = 0, high = -1;

        while (k--) {
            high = ni(n, high);
            sum += code[high];
        }

        for (int i = 0; i < n; i++) {
            res[low] = sum;
            low = ni(n, low);
            sum -= code[low];

            high = ni(n, high);
            sum += code[high];
        }

        if (negative) {
            reverse(res.begin(), res.end());
        }

        return res;
    }
};
```