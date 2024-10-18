---
difficulty: easy
leetcode-num: 20
topics:
  - Stack
  - String
---
[Problem Link](https://leetcode.com/problems/valid-parentheses)

#### Problem
Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

**Example 1:**

- **Input:** s = "()"

- **Output:** true

**Example 2:**

- **Input:** s = "()[]{}"

- **Output:** true

**Example 3:**

- **Input:** s = "(]"

- **Output:** false

**Example 4:**

- **Input:** s = "([])"

- **Output:** true

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of parentheses only `'()[]{}'`.

#### Solution
##### Optimal Approach
1. **Helper Function: `isBracketClosing(left, right)`**
    
    - **Purpose**: This function checks if a pair of brackets (one from the left and one from the right) form a valid closing pair.
    - **Logic**: It checks if the `left` bracket and `right` bracket form a valid pair by comparing three cases:
        - `(` and `)`
        - `{` and `}`
        - `[` and `]`
2. **Main Function: `isValid(s)`**
    
    - **Purpose**: This function checks if the string `s` has valid and balanced brackets.
    - **Logic**:
        - We use a **stack** to keep track of opening brackets.
        - **For each character `ch`** in the string:
            - If `ch` is an **opening bracket** (`(`, `{`, or `[`), we push it onto the stack.
            - If `ch` is a **closing bracket** (`)`, `}`, or `]`):
                1. We check if the stack is not empty (to ensure there’s an opening bracket to match the closing one).
                2. We pop the top element from the stack (which represents the most recent unmatched opening bracket) and check if it forms a valid pair with the current `ch` using `isBracketClosing()`.
                3. If it forms a valid pair, we remove it from the stack (by popping). If not, we return `false` because the string is not balanced.
        - **Final check**: After iterating through the entire string, the stack should be empty. If the stack is not empty, it means there are unmatched opening brackets left, so we return `false`.

###### Key Points:

- **Stack Usage**: The stack helps match each closing bracket with the most recent opening bracket, which ensures that brackets are closed in the correct order.
- **Valid pairs**: The `isBracketClosing` function ensures that only correct bracket pairs are matched (e.g., a `(` should close with a `)`).
- **Early Termination**: The function can return `false` early if it finds any unmatched closing bracket or if the brackets don’t close in the correct order.

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    bool isBracketClosing(char left, char right) {
        return (left == '(' && right == ')') || (left == '{' && right == '}') ||
               (left == '[' && right == ']');
    }
    
    bool isValid(string s) {
        stack<char> st;

        for (char ch : s) {
            if (ch == '(' || ch == '{' || ch == '[')
                st.push(ch);
            else {
                if (!st.empty() && isBracketClosing(st.top(), ch))
                    st.pop();
                else
                    return false;
            }
        }
        return st.empty();
    }
};
```