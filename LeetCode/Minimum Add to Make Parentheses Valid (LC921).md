---
difficulty: medium
leetcode-num: 921
topics:
  - Stack
  - String
  - Greedy
---
[Problem Link](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/)

#### Problem
A parentheses string is valid if and only if:

- It is the empty string,
- It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
- It can be written as `(A)`, where `A` is a valid string.

You are given a parentheses string `s`. In one move, you can insert a parenthesis at any position of the string.

- For example, if `s = "()))"`, you can insert an opening parenthesis to be `"(**(**)))"` or a closing parenthesis to be `"())**)**)"`.

Return _the minimum number of moves required to make_ `s` _valid_.

**Example 1:**

**Input:** s = "())"
**Output:** 1

**Example 2:**

**Input:** s = "((("
**Output:** 3

**Constraints:**

- `1 <= s.length <= 1000`
- `s[i]` is either `'('` or `')'`.

#### Solution
##### Optimal Approach (Greedy Approach)

- **Tracking opening and closing parentheses**:
    - We use two counters:
        - `opening`: Tracks the number of open parentheses `'('`.
        - `closing`: Tracks the number of close parentheses `')'`.
- **Iterating through the string**:  
    - For every character in the string `s`:
        - If it’s `'('`, increase the `opening` counter.
        - If it’s `')'`, increase the `closing` counter.
- **Handling imbalance**:
    - If at any point, `closing` exceeds `opening`, it means there's an extra closing parenthesis without a matching open one.
        - We increase the `addCount` by 1 to account for adding an open parenthesis.
        - Then we increment `opening` to artificially balance the current closing parenthesis.
- **Balancing leftover open parentheses**:
    - After iterating through the string, if there are any unmatched open parentheses (`opening > closing`), we need to add closing parentheses for each of them.
    - This is done by adding the difference `opening - closing` to `addCount`.
- **Return result**:
    - The total `addCount` gives us the minimum number of parentheses (either open or close) needed to make the string valid.


> [!info] Why balance `)` immediately?
> The closing bracket needs to be balanced immediately because a closing bracket without a preceding open bracket creates an imbalance that cannot be corrected by future open brackets; it represents an invalid state in the string at that moment.
> 
> By adding an open bracket right away, we ensure that each closing bracket has a valid match as soon as it's encountered.
> 
> In contrast, extra opening brackets can wait until the end because they remain valid as long as we eventually provide corresponding closing brackets, allowing us to balance them all at once after processing the entire string.


*TC ->* O( n )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    int minAddToMakeValid(string s) {
        int opening = 0, closing = 0, addCount = 0;

        for (char ch : s) {
            if (ch == '(')
                opening++;
            else
                closing++;

            // add open bracket for extra closing bracket as soon as found
            if (closing > opening) {
                addCount++;
                opening++;
            }
        }

        // add closing brackets for all extra opening brackets
        addCount += (opening - closing);

        return addCount;
    }
};
```

##### Optimal Approach (Imaginary Stack)
[Video Explanation](https://youtu.be/lIdHFWyIrYE)

1. **Imaginary stack**:
    
    - Instead of using an actual stack, we use the variable `stack_size` to represent the count of unmatched opening brackets `(` encountered in the string.
2. **Iterating through the string**:
    
    - For each character:
        - If it’s `'('`, we increment `stack_size`, representing pushing an open bracket onto the imaginary stack.
        - If it’s `')'`:
            - If `stack_size` is greater than 0 (meaning there's an unmatched `'('`), we decrement `stack_size`, simulating popping an open bracket to balance the current closing bracket.
            - If `stack_size` is 0, it means there are no unmatched open brackets to pair with this closing bracket, so we increment `addCount`, representing the need to add an extra open bracket.
3. **Balancing remaining opening brackets**:
    
    - After processing all characters, any unmatched opening brackets remaining in `stack_size` represent the number of open brackets without a matching close. To balance these, we add that number to `addCount`.
4. **Final result**:
    
    - `addCount` will give the total number of parentheses (both opening and closing) that need to be added to make the string valid.

###### Why This Works:

- **Immediate balancing**: Just like in the previous solution, the algorithm handles extra closing brackets right away because they can’t wait for future open brackets. By tracking the count of unmatched opening brackets using `stack_size`, the algorithm avoids unnecessary complexity.
- **End-of-string balancing**: Any remaining unmatched open brackets are handled at the end, similar to how the earlier solution adjusted for extra opening brackets in one final step.

*TC ->* O( n )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    int minAddToMakeValid(string s) {
        // imaginary stack where we will insert only opening brackets
        int stack_size = 0;
        int addCount = 0;

        for (char ch : s) {
            if (ch == '(')
                stack_size++;
            else if (stack_size ==
                     0) // Balance immediately if no matching open bracket
                addCount++;
            else
                stack_size--;
        }

        // add closing brackets for all remaining opening brackets in the stack
        addCount += stack_size;

        return addCount;
    }
};
```