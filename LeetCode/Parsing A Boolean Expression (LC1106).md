---
difficulty: hard
leetcode-num: 1106
topics:
  - Stack
  - String
  - Recursion
---
[Problem Link](https://leetcode.com/problems/parsing-a-boolean-expression/)

#### Problem
A **boolean expression** is an expression that evaluates to either `true` or `false`. It can be in one of the following shapes:

- `'t'` that evaluates to `true`.
- `'f'` that evaluates to `false`.
- `'!(subExpr)'` that evaluates to **the logical NOT** of the inner expression `subExpr`.
- `'&(subExpr1, subExpr2, ..., subExprn)'` that evaluates to **the logical AND** of the inner expressions `subExpr1, subExpr2, ..., subExprn` where `n >= 1`.
- `'|(subExpr1, subExpr2, ..., subExprn)'` that evaluates to **the logical OR** of the inner expressions `subExpr1, subExpr2, ..., subExprn` where `n >= 1`.

Given a string `expression` that represents a **boolean expression**, return _the evaluation of that expression_.

It is **guaranteed** that the given expression is valid and follows the given rules.

**Example 1:**

**Input:** expression = "&(|(f))"
**Output:** false
**Explanation:** 
First, evaluate |(f) --> f. The expression is now "&(f)".
Then, evaluate &(f) --> f. The expression is now "f".
Finally, return false.

**Example 2:**

**Input:** expression = "|(f,f,f,t)"
**Output:** true
**Explanation:** The evaluation of (false OR false OR false OR true) is true.

**Example 3:**

**Input:** expression = "!(&(f,t))"
**Output:** true
**Explanation:** 
First, evaluate &(f,t) --> (false AND true) --> false --> f. The expression is now "!(f)".
Then, evaluate !(f) --> NOT false --> true. We return true.

**Constraints:**

- `1 <= expression.length <= 2 * 104`
- expression[i] is one following characters: `'('`, `')'`, `'&'`, `'|'`, `'!'`, `'t'`, `'f'`, and `','`.

#### Solution

##### Optimal Approach
###### Detailed Explanation:

1. **Stacks**:
    
    - **`st1`**: This stack holds the boolean values `0` (for false) and `1` (for true) as well as a sentinel value `2` to mark the start of a sub-expression.
    - **`st2`**: This stack holds the operators `|`, `&`, and `!`.
2. **Core Logic**:
    
    - We iterate through the expression string character by character.
    - When we encounter an opening parenthesis `(`, we push the corresponding operator (before the `(`) onto `st2` and the sentinel `2` onto `st1`.
    - When we encounter a closing parenthesis `)`, it means that we have finished reading a sub-expression. Depending on the operator at the top of `st2`, we call a helper function (`insertOR`, `insertAND`, or `insertNOT`) to process the sub-expression and push the result (`0` or `1`) back onto `st1`.

###### Helper Functions:

- **`insertOR`**:
    
    - This function handles the `|` (OR) operator.
    - It evaluates the OR of all boolean values between the sentinel `2` and the top of the stack.
    - After calculating the result, it replaces the sub-expression with either `0` or `1` depending on whether the result was false or true.
- **`insertAND`**:
    
    - This function handles the `&` (AND) operator.
    - It evaluates the AND of all boolean values between the sentinel `2` and the top of the stack.
    - Like `insertOR`, it pops the sub-expression and replaces it with `0` or `1` based on the result.
- **`insertNOT`**:
    
    - This function handles the `!` (NOT) operator.
    - It negates the top boolean value on `st1` and replaces the sub-expression with `0` or `1`.

###### Main Loop:

- **If `(` is found**: Push the operator before it (either `|`, `&`, or `!`) onto `st2` and `2` onto `st1` to mark the start of a sub-expression.
    
- **If `)` is found**: This indicates the end of a sub-expression. Based on the operator at the top of `st2`, we call the corresponding helper function to evaluate the sub-expression and push the result back onto `st1`.
    
- **If `t` (true) is found**: Push `1` onto `st1`.
    
- **If `f` (false) is found**: Push `0` onto `st1`.


*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {
    void insertOR(stack<int>& st) {
        bool res = false;

        while (st.top() != 2) {
            res |= st.top();
            st.pop();
        }

        st.pop(); // remove 2
        st.push(res ? 1 : 0);
    }

    void insertAND(stack<int>& st) {
        bool res = true;

        while (st.top() != 2) {
            res &= st.top();
            st.pop();
        }

        st.pop(); // remove 2
        st.push(res ? 1 : 0);
    }

    void insertNOT(stack<int>& st) {

        bool res = !st.top();

        st.pop(); // remove operand
        st.pop(); // remove 2
        st.push(res ? 1 : 0);
    }

public:
    bool parseBoolExpr(string exp) {
        stack<int> st1;
        stack<char> st2;

        for (int i = 0; i < exp.size(); i++) {
            char ch = exp[i];
            if (ch == '(') {
                st2.push(exp[i - 1]);
                st1.push(2);

            }

            else if (ch == ')') {
                if (st2.top() == '|') {
                    insertOR(st1);
                }

                else if (st2.top() == '&') {
                    insertAND(st1);
                }

                else if (st2.top() == '!') {
                    insertNOT(st1);
                }

                st2.pop();
            }

            else if (ch == 'f') {
                st1.push(0);
            }

            else if (ch == 't') {
                st1.push(1);
            }
        }

        return st1.top();
    }
};
```