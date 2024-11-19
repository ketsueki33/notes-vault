A stack is a linear data structure that follows the **Last-In-First-Out (LIFO)** principle. It behaves like a stack of plates, where the last plate added is the first one to be removed.

![[../_assets/stack_vis.png|460]]
#### Basic Operations of Stack
###### 1. `push( )`
Inserts an item to the top of the stack.

###### 2. `pop( )`
Removes an item from the top of the stack.  Throws error if stack is empty.

###### 3. `empty( )`
Returns true if the stack is empty others

###### 4. `top( )`/`peek( )
Returns the top element currently in the stack. Throws error if stack is empty.

###### 5. `size( )`
Returns the number of elements currently in the stack.

#### Corner Conditions
##### Stack Underflow
An error condition that occurs when an item is called for from the stack, but the stack is empty.

**Eg)** Using `pop()` or `top()` on an empty stack.

##### Stack Overflow
An error condition that occurs if the call stack pointer exceeds the stack bound. This error is not common as  STL implementations of Stack are dynamic sized so there is no overflow.

#### Applications of Stack
- Recursive function calls.
- Balanced parenthesis.
- Reversing items.
- In-fix expression to post-fix
- Evaluation of In-fix and Post-fix expressions.
- Stock Span problems
- Undo / Redo operations.A stack overflow occurs if the call stack pointer exceeds the stack bound. The call stack may consist of a limited amount of address space.

#### Implementing Stack using Array
```cpp
class MyStack {
    int arr[1000];
    int top;

   public:
    MyStack() { top = -1; }

    void push(int x) {
        top++;
        arr[top] = x;
    }

    int pop() {
        if (top == -1)
            return -1;
        return arr[top--];
    }
};
```

#### Stack in C++ STL
```cpp
stack<data_type> st;
```

- Stacks are implemented as a **container adapter** in C++.

> [!info] Container Adapters
> In C++, container adapters are specialized interfaces created on top of other sequential containers like `deque`, or `list` by limiting functionality in a pre-existing container and providing more specific functionalities. It is done so as to avoid defining a completely new interface for containers that can be built on top of the already existing containers.

- Stack can be initialized with dequeue, list or vector as the container. Default is *deque*.
- All stack operations (push, pop, top, empty , size ) take O( 1 ) time.

#### Monotonic Stack
A monotonic stack is *a special type of stack that keeps its elements in a specific order, either increasing or decreasing.*

Monotonic stacks are useful for solving problems that require finding the next larger or smaller element in a sequence. This is often called a Next Greater Element (NGE) or Next Smaller Element (NSE) problem. 

**Here's how a monotonic stack works:**
- Before pushing a new element, the stack checks if it would break the monotonic condition.

- If it would, the stack pops the top element until pushing the new element no longer breaks the condition.

We will understand Monotonic Stack by looking at some problems.

##### Next Greater Element
*Problem Statement:* Given an array of integers, find the next greater element for each element. If there is no greater element, return `-1`.

We solve this by maintaining a monotonic stack that holds element in decreasing order while traversing the array from the back.

**In each iteration we:**
- At each step, we compare the current element with the **top** of the stack.
- If the current element is **smaller than**  the top of the stack, we **push** it onto the stack. This maintains the strictly decreasing order, as the smaller element can potentially find its NGE from elements pushed onto the stack later.
- If the current element is **greater than** the top of the stack, we **pop** elements from the stack until the stack is either empty or we find a greater element. This step ensures that the stack only holds potential NGEs (next greater elements) for future elements.

**If Stack is Empty**:

- If the stack is empty at any point during this process, it means that there is **no greater element** to the right of the current element. In this case, we simply store `-1` as the NGE for the current element.
- For example, at the rightmost element of the array, the stack is always empty because there is no element to its right, so its NGE is `-1`.

![[../_assets/Pasted image 20241016134000.png|550]]

[Video Explanation](https://youtu.be/e7XQLtOQM3I)

```cpp title="Code Implementation"
vector<int> nextGreaterElement(vector<int>& nums) {
    vector<int> res(nums1.size(), -1);
    stack<int> st;

    for (int i = nums.size() - 1; i >= 0; i--) {
        int num = nums[i];

        while (!st.empty() && num >= st.top())
            st.pop();

        if (!st.empty()) {
            res[i] = st.top();
        }

        st.push(num);
    }
    return res;
}
```

Also see: [[../Practice/Stacks/Next Greater Element I (LC496)|Next Greater Element I (LC496)]] , [[../Practice/Stacks/Next Greater Element II (LC503)|Next Greater Element II (LC503)]]