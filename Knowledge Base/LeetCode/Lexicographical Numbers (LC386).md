---
difficulty: medium
leetcode-num: 386
topics:
  - Depth-First Search
  - Trie
---
[Problem Link](https://leetcode.com/problems/lexicographical-numbers/)

#### Problem
Given an integer `n`, return all the numbers in the range `[1, n]` sorted in lexicographical order.

You must write an algorithm that runs in `O(n)` time and uses `O(1)` extra space. 

**Example 1:**

**Input:** n = 13
**Output:** `[1,10,11,12,13,2,3,4,5,6,7,8,9]`

**Example 2:**

**Input:** n = 2
**Output:** `[1,2]`

**Constraints:**

- `1 <= n <= 5 * 104`

#### Solution


##### DFS - Optimal Approach
[Video Explanation](https://youtu.be/vuS3US_bDBo)

- **Recursive `solve` Function**:    
    - This function takes the current number (`currNum`), the limit (`n`), and the result vector (`res`).
    - **Base Case**: If `currNum` exceeds `n`, the function returns immediately since there’s no need to process numbers larger than `n`.
    - **Add the Number to Result**: Every valid number (i.e., `currNum` ≤ `n`) is added to the result vector `res`.
    - **Explore the Next Numbers**: The loop inside the function generates the next number by appending digits (from 0 to 9) to the current number and recursively calls the `solve` function to continue this process. This builds lexicographically smaller numbers first, ensuring the correct order.
- **`lexicalOrder` Function**:   
    - This is the main function that drives the solution.
    - It initializes the result vector `res`, which will store all the numbers in lexicographical order.
    - **Starting the DFS**: A loop from 1 to 9 starts the DFS process for each digit. The recursive `solve` function is then called with the initial `currNum` being each of these digits. This is because lexicographically, the numbers start with 1, 2, ..., 9.
    - Once all numbers starting with these digits are generated, the result vector `res` is returned.

*TC ->* O( n )
*SC ->* O( n log$_{10}$ n ) , if we count recursion stack space

```cpp title=Code
class Solution {
    void solve(int currNum, int limit, vector<int>& res) {
        if (currNum > limit)
            return;

        res.push_back(currNum);

        for (int  i = 0; i <= 9; i++) {
            int nextNum = currNum * 10 + i;
            solve(nextNum, limit, res);
        }
    }

public:
    vector<int> lexicalOrder(int n) {
        vector<int> res;

        for (int start = 1; start <= 9; start++)
            solve(start, n, res);

        return res;
    }
};
```

##### Brute Force Approach

- Create a vector with all numbers upto `n`. 
- Create a custom comparison function that converts int to string and determines which number is lexicographically lower.
- Use the sort operation on the vector with the custom comparison function.
- Return the result.

*TC ->* O( `n * log n` )
*SC ->* O( 1 ) , excluding result vector

```cpp title=Code
class Solution {
    static bool myCmp(int a, int b) { return to_string(a) < to_string(b); }

public:
    vector<int> lexicalOrder(int n) {
        vector<int> v;

        for (int i = 1; i <= n; i++)
            v.push_back(i);

        sort(v.begin(), v.end(), myCmp);

        return v;
    }
};
```