---
difficulty: medium
leetcode-num: 641
topics:
  - Array
  - Queue
  - Linked List
  - Design
---
[Problem Link](https://leetcode.com/problems/design-circular-deque/)

#### Problem
Design your implementation of the circular double-ended queue (deque).

Implement the `MyCircularDeque` class:

- `MyCircularDeque(int k)` Initializes the deque with a maximum size of `k`.
- `boolean insertFront()` Adds an item at the front of Deque. Returns `true` if the operation is successful, or `false` otherwise.
- `boolean insertLast()` Adds an item at the rear of Deque. Returns `true` if the operation is successful, or `false` otherwise.
- `boolean deleteFront()` Deletes an item from the front of Deque. Returns `true` if the operation is successful, or `false` otherwise.
- `boolean deleteLast()` Deletes an item from the rear of Deque. Returns `true` if the operation is successful, or `false` otherwise.
- `int getFront()` Returns the front item from the Deque. Returns `-1` if the deque is empty.
- `int getRear()` Returns the last item from Deque. Returns `-1` if the deque is empty.
- `boolean isEmpty()` Returns `true` if the deque is empty, or `false` otherwise.
- `boolean isFull()` Returns `true` if the deque is full, or `false` otherwise.

**Example 1:**

**Input**
`["MyCircularDeque", "insertLast", "insertLast", "insertFront", "insertFront", "getRear", "isFull", "deleteLast", "insertFront", "getFront"]`
`[[3], [1], [2], [3], [4], [], [], [], [4], []]`
**Output**
`[null, true, true, true, false, 2, true, true, true, 4]`

**Explanation**
MyCircularDeque myCircularDeque = new MyCircularDeque(3);
myCircularDeque.insertLast(1);  // return True
myCircularDeque.insertLast(2);  // return True
myCircularDeque.insertFront(3); // return True
myCircularDeque.insertFront(4); // return False, the queue is full.
myCircularDeque.getRear();      // return 2
myCircularDeque.isFull();       // return True
myCircularDeque.deleteLast();   // return True
myCircularDeque.insertFront(4); // return True
myCircularDeque.getFront();     // return 4

**Constraints:**

- `1 <= k <= 1000`
- `0 <= value <= 1000`
- At most `2000` calls will be made to `insertFront`, `insertLast`, `deleteFront`, `deleteLast`, `getFront`, `getRear`, `isEmpty`, `isFull`.

#### Solution
##### Using List
```cpp title=Code
class MyCircularDeque {
    list<int> dq;
    int k;

public:
    MyCircularDeque(int k) { this->k = k; }

    bool insertFront(int value) {
        if (isFull())
            return false;

        dq.push_front(value);

        return true;
    }

    bool insertLast(int value) {
        if (isFull())
            return false;

        dq.push_back(value);
        return true;
    }

    bool deleteFront() {
        if (isEmpty())
            return false;

        dq.pop_front();
        return true;
    }

    bool deleteLast() {
        if (isEmpty())
            return false;

        dq.pop_back();
        return true;
    }

    int getFront() {
        if (isEmpty())
            return -1;

        return dq.front();
    }

    int getRear() {
        if (isEmpty())
            return -1;

        return dq.back();
    }

    bool isEmpty() {
        if (dq.size() == 0)
            return true;
        return false;
    }

    bool isFull() {
        if (dq.size() == k)
            return true;
        return false;
    }
};
```

##### Using Array

```cpp title=Code
class MyCircularDeque {
    int* dq;
    int front, maxSize, rear, currSize;

    int prev(int i) { return (i - 1 + maxSize) % maxSize; }

    int next(int i) { return (i + 1) % maxSize; }

public:
    MyCircularDeque(int k) {
        dq = new int[k];
        front = 0;
        rear = 0;
        currSize = 0;
        maxSize = k;
    }

    bool insertFront(int value) {
        if (isFull())
            return false;

        front = prev(front);
        dq[front] = value;
        currSize++;

        if (currSize == 1)
            rear = front;

        return true;
    }

    bool insertLast(int value) {
        if (isFull())
            return false;

        rear = next(rear);
        dq[rear] = value;
        currSize++;

        if (currSize == 1)
            front = rear;

        return true;
    }

    bool deleteFront() {
        if (isEmpty())
            return false;
        front = next(front);
        currSize--;

        return true;
    }

    bool deleteLast() {
        if (isEmpty())
            return false;
        rear = prev(rear);
        currSize--;

        return true;
    }

    int getFront() {
        if (isEmpty())
            return -1;
        return dq[front];
    }

    int getRear() {
        if (isEmpty())
            return -1;
        return dq[rear];
    }

    bool isEmpty() {
        if (currSize == 0)
            return true;
        return false;
    }

    bool isFull() {
        if (currSize == maxSize)
            return true;
        return false;
    }
};
```