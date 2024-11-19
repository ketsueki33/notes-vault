---
difficulty: easy
leetcode-num: 225
topics:
  - Stack
  - Design
  - Queue
---
[Problem Link](https://leetcode.com/problems/implement-stack-using-queues/)

#### Problem
Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (`push`, `top`, `pop`, and `empty`).

Implement the `MyStack` class:

- `void push(int x)` Pushes element x to the top of the stack.
- `int pop()` Removes the element on the top of the stack and returns it.
- `int top()` Returns the element on the top of the stack.
- `boolean empty()` Returns `true` if the stack is empty, `false` otherwise.

**Notes:**

- You must use **only** standard operations of a queue, which means that only `push to back`, `peek/pop from front`, `size` and `is empty` operations are valid.
- Depending on your language, the queue may not be supported natively. You may simulate a queue using a list or deque (double-ended queue) as long as you use only a queue's standard operations.

**Example 1:**

**Input**
`["MyStack", "push", "push", "top", "pop", "empty"]`
`[[], [1], [2], [], [], []]`
**Output**
`[null, null, null, 2, 2, false]`

**Explanation**
MyStack myStack = new MyStack();
myStack.push(1);
myStack.push(2);
myStack.top(); // return 2
myStack.pop(); // return 2
myStack.empty(); // return False

**Constraints:**

- `1 <= x <= 9`
- At most `100` calls will be made to `push`, `pop`, `top`, and `empty`.
- All the calls to `pop` and `top` are valid.

**Follow-up:** Can you implement the stack using only one queue?

#### Solution
##### Single Queue Approach
- We maintain a single queue `que`.
- When pushing a new element, we first push it onto the queue. Then, we rotate the queue by pushing and popping elements until the newly pushed element becomes the front.
- Pop, top, and empty operations are straightforward, as they directly operate on the single queue.

*TC ->* 
- Push: ( O(n) ) - Rotating the queue to bring the newly pushed element to the front.
- Pop, Top, Empty: ( O(1) )

*SC ->* O( n )

```cpp title=Code
class MyStack {
    queue<int> q;

public:
    MyStack() {}

    void push(int x) {
        q.push(x);

        for (int i = 0; i < q.size() - 1; i++) {
            q.push(q.front());
            q.pop();
        }
    }

    int pop() {
        int temp = q.front();
        q.pop();
        return temp;
    }

    int top() { return q.front(); }

    bool empty() { return q.empty(); }
};

```

##### Two Queues Approach
- We maintain two queues, `que1` and `que2`.
- When pushing a new element, we push it onto `que2` and then move all elements from `que1` to `que2`, effectively making the newly pushed element at the front of `que2`. Then, we swap the names of `que1` and `que2`.
- Pop, top, and empty operations are straightforward, as they directly operate on `que1`.

*TC ->* 
- Push: ( O(n) ) - Moving all elements from one queue to another.
- Pop, Top, Empty: ( O(1) )

*SC ->* O( n )

```cpp title=Code
class MyStack {
    queue<int> q1, q2;

public:
    MyStack() {}

    void push(int x) {
        q2.push(x);
        while (!q1.empty()) {
            q2.push(q1.front());
            q1.pop();
        }

        swap(q1, q2);
    }

    int pop() {

        int temp = q1.front();
        q1.pop();
        return temp;
    }

    int top() { return q1.front(); }

    bool empty() { return q1.empty(); }
};
```