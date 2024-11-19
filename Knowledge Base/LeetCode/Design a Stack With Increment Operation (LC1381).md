---
difficulty: medium
leetcode-num: 1381
topics:
  - Design
  - Stack
---
[Problem Link](https://leetcode.com/problems/design-a-stack-with-increment-operation)

#### Problem
Design a stack that supports increment operations on its elements.

Implement the `CustomStack` class:

- `CustomStack(int maxSize)` Initializes the object with `maxSize` which is the maximum number of elements in the stack.
- `void push(int x)` Adds `x` to the top of the stack if the stack has not reached the `maxSize`.
- `int pop()` Pops and returns the top of the stack or `-1` if the stack is empty.
- `void inc(int k, int val)` Increments the bottom `k` elements of the stack by `val`. If there are less than `k` elements in the stack, increment all the elements in the stack.

**Example 1:**

**Input**
`["CustomStack","push","push","pop","push","push","push","increment","increment","pop","pop","pop","pop"]`
`[[3],[1],[2],[],[2],[3],[4],[5,100],[2,100],[],[],[],[]]`
**Output**
`[null,null,null,2,null,null,null,null,null,103,202,201,-1]`
**Explanation**
CustomStack stk = new CustomStack(3); // Stack is Empty []
stk.push(1);                          // stack becomes `[1]`
stk.push(2);                          // stack becomes `[1, 2]`
stk.pop();                            // return 2 --> Return top of the stack 2, stack becomes `[1]`
stk.push(2);                          // stack becomes `[1, 2]`
stk.push(3);                          // stack becomes `[1, 2, 3]`
stk.push(4);                          // stack still `[1, 2, 3]`, Do not add another elements as size is 4
stk.increment(5, 100);                // stack becomes `[101, 102, 103]`
stk.increment(2, 100);                // stack becomes `[201, 202, 103]`
stk.pop();                            // return 103 --> Return top of the stack 103, stack becomes `[201, 202]`
stk.pop();                            // return 202 --> Return top of the stack 202, stack becomes` [201]`
stk.pop();                            // return 201 --> Return top of the stack 201, stack becomes `[]`
stk.pop();                            // return -1 --> Stack is empty return -1.

**Constraints:**

- `1 <= maxSize, x, k <= 1000`
- `0 <= val <= 100`
- At most `1000` calls will be made to each method of `increment`, `push` and `pop` each separately.

#### Solution
[Video Explanation](https://youtu.be/1KYzjryTRmg)

##### O(1) increment Approach
To achieve O( 1 ) increment, we will use the Lazy Propagation technique.

###### Key Idea:
- Instead of incrementing each of the first `k` elements directly, we store the increment amount in the `increments` array at index `k - 1`. When an element is popped, we apply the stored increment to the value being popped and carry over the increment to the next element below.

###### Breakdown:
 1. **Push Operation**:

- Standard push to the stack. If the stack isn't full, the new element is placed on top of the stack (in the `st` array), and no changes are made to the `increments` array during this operation.

2. **Pop Operation**:

- When popping, instead of just returning the top element, we also consider any increments that have been deferred via the `increments` array.
    - **Retrieve the top value**: We retrieve the value at `st[top]`.
    - **Apply increment**: We add the corresponding value from `increments[top]` to the element.
    - **Reset the increment**: Once the increment is applied, we set `increments[top]` to `0`, as the increment has been "used up".
    - **Carry forward any remaining increments**: If the stack still has elements after the pop (i.e., `top != -1`), we propagate the increment from the popped element down to the next element (`increments[top - 1]`), ensuring future pops correctly account for the increment.

3. **Increment Operation**:

- Instead of directly incrementing the first `k` elements, we only increment the `increments[k-1]` value. This stores the amount that will be added to all future pops of the first `k` elements.
    - When `k` is greater than the current stack size, we clamp `k` to `top + 1` to avoid over-incrementing non-existent elements.

*TC ->* O( 1 )
*SC ->* O( n )

```cpp title=Code
class CustomStack {
    int* st;
    int* increments;
    int maxSize;
    int top;

public:
    CustomStack(int maxSize) {
        st = new int[maxSize];
        increments = new int[maxSize]();
        this->maxSize = maxSize;
        top = -1;
    }

    void push(int x) {
        if (top == maxSize - 1)
            return;
        top++;

        st[top] = x;
    }

    int pop() {
        if (top == -1)
            return -1;
        int val = st[top];
        int add = increments[top];
        increments[top] = 0;
        val += add;

        top--;
        if (top != -1)
            increments[top] += add;

        return val;
    }

    void increment(int k, int val) {
        k = min(k - 1, top);
        if (k >= 0)
            increments[k] += val;
    }
};
```

##### O(k) increment Approach

*TC ->* O( 1 ) for push,pop ; O( k ) for increment
*SC ->* O( n )

```cpp title=Code
class CustomStack {
    int* st;
    int maxSize;
    int top;
public:
    CustomStack(int maxSize) {
        st = new int[maxSize];
        this->maxSize = maxSize;
        top = -1;
    }
    
    void push(int x) {
        if( top == maxSize - 1 )
            return;
        top++;
        st[top] = x;
    }
    
    int pop() {
        if(top == -1 )
            return -1;
        return st[top--];
    }
    
    void increment(int k, int val) {
        for( int i = 0 ; i < k && i <= top ; i++)
            st[i]+= val;
    }
};
```