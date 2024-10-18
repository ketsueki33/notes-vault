---
difficulty: easy
leetcode-num: 2696
topics:
  - String
  - Stack
  - Simulation
  - Two Pointer
---
[Problem Link](https://leetcode.com/problems/minimum-string-length-after-removing-substrings/)

#### Problem
You are given a string `s` consisting only of **uppercase** English letters.

You can apply some operations to this string where, in one operation, you can remove **any** occurrence of one of the substrings `"AB"` or `"CD"` from `s`.

Return _the **minimum** possible length of the resulting string that you can obtain_.

**Note** that the string concatenates after removing the substring and could produce new `"AB"` or `"CD"` substrings.

**Example 1:**

**Input:** s = "ABFCACDB"
**Output:** 2
**Explanation:** We can do the following operations:
- Remove the substring "ABFCACDB", so s = "FCACDB".
- Remove the substring "FCACDB", so s = "FCAB".
- Remove the substring "FCAB", so s = "FC".
So the resulting length of the string is 2.
It can be shown that it is the minimum length that we can obtain.

**Example 2:**

**Input:** s = "ACBBD"
**Output:** 5
**Explanation:** We cannot do any operations on the string so the length remains the same.

**Constraints:**

- `1 <= s.length <= 100`
- `s` consists only of uppercase English letters.

#### Solution
[Video Explanation](https://youtu.be/4XLzLdAE4Lc)

##### Optimal Approach ( Two Pointer )
###### Key Concepts:

1. **Two Pointer Technique**:
    
    - **Pointer `i` (write pointer)**: It tracks the position where the next valid character will be written.
    - **Pointer `j` (read pointer)**: It scans through the string, reading each character one by one.
    - This method helps in-place modification of the string as you process it, without the need for extra space.
2. **Checking for "AB" and "CD" Patterns**:
    
    - As you read through the string with pointer `j`, you keep track of the last character written with pointer `i`.
    - If you find a valid pair (either "AB" or "CD"), you remove it by decrementing the `i` pointer (essentially removing the last written character).
    - If no valid pair is found, the character read by `j` is written to the position `i` points to, and you move both pointers forward.
3. **Simulating Stack Behavior**:
    
    - By using the write pointer `i`, the solution mimics the behavior of a stack. When a valid pair like "AB" or "CD" is encountered, the last character is "popped" by decrementing `i`.
    - If no pair is formed, the current character is "pushed" to the position pointed by `i`.

###### Explanation of the Solution:

1. **Initialization**:
    
    - `i = 0` starts at the first character (write pointer).
    - `j = 1` starts at the second character (read pointer).
    - The process begins by reading the second character because we compare pairs (current character and the previous one).
2. **Processing the String**:
    
    - The `while` loop runs as long as `j` is less than the length of the string `n`.
        
    - Three cases are checked inside the loop:
        
        - **Case 1**: `i < 0`:
            
            - This means we have "popped" everything and `i` went below zero, so we reset `i` to 0 and assign `s[i] = s[j]` to start fresh.
        - **Case 2**: Pair Found (`s[i] == 'A' && s[j] == 'B'` or `s[i] == 'C' && s[j] == 'D'`):
            
            - If the last written character (`s[i]`) forms a valid pair with the current character (`s[j]`), decrement `i` to effectively remove the last written character.
            - This simulates "popping" the valid pair from the stack.
        - **Case 3**: No Pair Found:
            
            - If no valid pair is formed, we move the write pointer `i` forward, write the current character (`s[j]`) to position `i`, and continue.
3. **Final Return**:
    
    - After the loop completes, the length of the remaining valid string is `i + 1`. This is because `i` is the index of the last valid character, so the length is `i + 1`.

*TC ->* O( n )
*SC ->* O( 1 )

```cpp title=Code
class Solution {

public:
    int minLength(string s) {
        int n = s.size();

        int i = 0; // write
        int j = 1; // read

        while (j < n) {
            if (i < 0) {
                i++;
                s[i] = s[j];
            } else if ((s[i] == 'A' && s[j] == 'B') ||
                       (s[i] == 'C' && s[j] == 'D')) {
                i--;
            } else {
                i++;
                s[i] = s[j];
            }
            j++;
        }
        return i + 1;
    }
};
```

##### Better Approach (Stack)

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {

public:
    int minLength(string s) {
        stack<char> st;

        for (char ch : s) {
            if (!st.empty()) {
                if (st.top() == 'A' && ch == 'B') {
                    st.pop();
                    continue;
                }
                if (st.top() == 'C' && ch == 'D') {
                    st.pop();
                    continue;
                }
            }
            st.push(ch);
        }

        return st.size();
    }
};
```