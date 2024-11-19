---
difficulty: medium
leetcode-num: 1963
topics:
  - Stack
  - String
  - Greedy
---
[Problem Link](https://leetcode.com/problems/minimum-number-of-swaps-to-make-the-string-balanced/)

#### Problem
You are given a **0-indexed** string `s` of **even** length `n`. The string consists of **exactly** `n / 2` opening brackets `'['` and `n / 2` closing brackets `']'`.

A string is called **balanced** if and only if:

- It is the empty string, or
- It can be written as `AB`, where both `A` and `B` are **balanced** strings, or
- It can be written as `[C]`, where `C` is a **balanced** string.

You may swap the brackets at **any** two indices **any** number of times.

Return _the **minimum** number of swaps to make_ `s` _**balanced**_.

**Example 1:**

**Input:** `s = "][]["`
**Output:** 1
**Explanation:** You can make the string balanced by swapping index 0 with index 3.
The resulting string is `"[[]]"`.

**Example 2:**

**Input:** s = `"]]][[["`
**Output:** 2
**Explanation:** You can do the following to make the string balanced:
- Swap index 0 with index 4. s = `"[]][]["`.
- Swap index 1 with index 5. s = `"[[][]]"`.
The resulting string is `"[[][]]"`.

**Example 3:**

**Input:** s = `"[]"`
**Output:** 0
**Explanation:** The string is already balanced.

**Constraints:**

- `n == s.length`
- `2 <= n <= 106`
- `n` is even.  
- `s[i]` is either `'['` or `']'`.
- The number of opening brackets `'['` equals `n / 2`, and the number of closing brackets `']'` equals `n / 2`.

#### Solution
##### Optimal Approach
1. **Variables to Track Bracket Counts**
	
	- `opening`: Tracks the number of opening brackets (`[`).
	- `closing`: Tracks the number of closing brackets (`]`).
	- `swapCount`: Tracks the number of swaps needed to balance the string.

2. **Iterating Through the String**
	
	- For each character in the string:
	    - If it's an **opening bracket** (`[`), increment the `opening` counter.
	    - If it's a **closing bracket** (`]`), increment the `closing` counter.

3. **Handling Imbalance**

	- If at any point during the iteration, the `closing` count exceeds the `opening` count, it indicates an imbalance (too many closing brackets without enough opening brackets to match them).
	- **Swap Logic**:
	    - To correct this imbalance, a swap is performed, which conceptually "moves" an opening bracket to match the extra closing bracket.
	    - This is done by incrementing the `opening` count (since after the swap, there will be one more opening bracket) and decrementing the `closing` count (since the extra closing bracket is now balanced).

4. **Returning the Swap Count**
		
	- After iterating through the string, the total number of swaps needed to balance the brackets is stored in `swapCount`, which is returned as the result.
###### Why This Approach Works?
- Since the number of opening and closing brackets is guaranteed to be equal, every time there’s an imbalance (i.e., more closing brackets than opening brackets), you can perform a swap to balance them.
- The swap increases the number of opening brackets by 1 and decreases the number of unbalanced closing brackets by 1.
- By the end of the string, all opening and closing brackets will be perfectly matched.

*TC ->* O( n )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    int minSwaps(string s) {
        int opening = 0, closing = 0, swapCount = 0;

        for (char ch : s) {
            if (ch == '[')
                opening++;
            if (ch == ']')
                closing++;
            if (closing > opening) {
                swapCount++;
                opening++;
                closing--;
            }
        }

        return swapCount;
    }
};
```

##### Good Approach (Stack)
[Video Explanation](https://youtu.be/W61jIP-O8lw)

1. **Stack to Track Unbalanced Brackets**
	
	- A stack (`st`) is used to keep track of unmatched open brackets (`[`).
	- **Goal**: To pair every closing bracket (`]`) with an open bracket (`[`).

2. **Iterating Over the String**

	- For each character in the string `s`:
	    - If the character is an open bracket (`[`), it's pushed onto the stack.
	    - If it's a closing bracket (`]`) and the stack is not empty (meaning there's an unpaired open bracket), the top of the stack is popped to "balance" the closing bracket. This indicates an open and closing pair has been matched.

3. **Unbalanced Open Brackets in the Stack**
	
	- After processing the entire string, the stack will only contain unmatched open brackets that couldn't be paired with a closing bracket.
	- To balance these remaining brackets, we need to swap some characters.

4. **Number of Swaps Needed**
	
	- Every two unbalanced open brackets can be balanced by swapping them with the necessary closing brackets. So, the number of swaps required is `(st.size() + 1) / 2`.
	    - The `+1` ensures rounding up in case there's an odd number of unmatched brackets, as one additional swap will always be needed in such cases.

###### Summary of Key Steps:
1. Push open brackets onto the stack.
2. Pop the stack for every closing bracket when there's a matching open bracket.
3. After processing, calculate the swaps needed to balance remaining unmatched open brackets.


> [!info] Simulate Stack
> We can use this approach without stack by just keeping track of the size of an imaginary stack.
> That would make this a O( 1 ) space complexity solution.


*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    int minSwaps(string s) {
        stack<char> st;

        for (char ch : s) {
            if (ch == '[')
                st.push(ch);
            else if (!st.empty())
                st.pop(); // balance closing bracket with an open bracket
        }

        return (st.size() + 1) / 2;
    }
};

```
