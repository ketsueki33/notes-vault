---
topics:
  - Stack
  - Recursion
difficulty: medium
---
[Problem Link](https://www.geeksforgeeks.org/problems/sort-a-stack/1)

#### Problem
Given a stack, the task is to sort it such that the top of the stack has the greatest element.

**Example 1:**

**Input:**
Stack: 3 2 1
**Output:** 3 2 1

**Example 2:**

**Input:**
Stack: 11 2 32 3 41
**Output:** 41 32 11 3 2

**Your Task:**   
You don't have to read input or print anything. Your task is to complete the function **sort()** which sorts the elements present in the given stack. (The sorted stack is printed by the driver's code by popping the elements of the stack.)

**Expected Time Complexity**: O(N*N)  
**Expected Auxilliary Space**: O(N) recursive.

**Constraints:**  
1<=N<=100

#### Solution
[Video Explanation](https://youtu.be/XNAv8NrAwng)

##### Optimal Approach
###### Approach Breakdown:
1. **`insertCorrectPos(x, s)` function**:
    
    - **Purpose**: To insert the element `x` into its correct position in an already sorted stack `s`.
    - **Logic**:
        - If the stack is empty or the top element of the stack is smaller than `x`, it means `x` can safely be pushed to the top.
        - Otherwise:
            1. Pop the top element and store it in `y`.
            2. Recursively call `insertCorrectPos` to find the correct position for `x`.
            3. After `x` is inserted, push `y` back on top.
        - This effectively "pushes" elements upwards, making room for `x` to be inserted in the correct spot.
2. **`sort()` function**:
    
    - **Purpose**: To sort the entire stack using recursion.
    - **Logic**:
        - Base case: If the stack is empty, return (i.e., the sorting is done).
        - Otherwise:
            1. Pop the top element (`x`) of the stack.
            2. Recursively call `sort()` to sort the remaining stack.
            3. Once the rest of the stack is sorted, use `insertCorrectPos` to put `x` in the correct place in the now sorted stack.

###### Step-by-step Execution:

1. The function `sort()` pops elements one by one from the stack until it's empty, calling itself recursively. The stack becomes empty at some point due to this process.
    
2. Once the stack is empty, the recursion starts unwinding:
    
    - At each step, the function `insertCorrectPos()` is called to place the popped element in its correct position relative to the sorted portion of the stack.
3. **Recursive Insertion**:
    
    - For each element, `insertCorrectPos()` checks if the stack’s top element is smaller. If so, it pushes the element onto the stack. If not, it pops the top element to make space, inserts the element in the right position, and then pushes back the removed element.

###### Key Points:

- **Recursive sorting**: The `sort()` function sorts the stack by breaking it down to smaller problems recursively.
- **Recursive insertion**: `insertCorrectPos` ensures that each element is placed correctly when the stack is partially sorted.
- **Unwinding phase**: Once the stack is empty, the recursion unwinds, and the elements are placed back in sorted order.

*TC ->* O(n^2)
*SC ->* O( n )

```cpp title=Code
void insertCorrectPos(int x, stack<int> &s){
    if( s.empty() || s.top() < x )
        s.push(x);
    else{
        int y = s.top();
        s.pop();
        insertCorrectPos(x,s);
        s.push(y);
    }
}

void SortedStack :: sort()
{
    if( s.empty() )
        return;
    
    int x = s.top();
    s.pop();
    sort();
    insertCorrectPos(x,s);
    
}
```