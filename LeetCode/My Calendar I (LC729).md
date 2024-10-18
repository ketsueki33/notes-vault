---
difficulty: medium
leetcode-num: 729
topics:
  - Array
  - Ordered Set
  - Binary Search
  - Intervals
---
[Problem Link](https://leetcode.com/problems/my-calendar-i/description/)

#### Problem
You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a **double booking**.

A **double booking** happens when two events have some non-empty intersection (i.e., some moment is common to both events.).

The event can be represented as a pair of integers `start` and `end` that represents a booking on the half-open interval `[start, end)`, the range of real numbers `x` such that `start <= x < end`.

Implement the `MyCalendar` class:

- `MyCalendar()` Initializes the calendar object.
- `boolean book(int start, int end)` Returns `true` if the event can be added to the calendar successfully without causing a **double booking**. Otherwise, return `false` and do not add the event to the calendar.

**Example 1:**

**Input**
`["MyCalendar", "book", "book", "book"]`
`[[], [10, 20], [15, 25], [20, 30]]`
**Output**
`[null, true, false, true]`

**Explanation**
MyCalendar myCalendar = new MyCalendar();
myCalendar.book(10, 20); // return True
myCalendar.book(15, 25); // return False, It can not be booked because time 15 is already booked by another event.
myCalendar.book(20, 30); // return True, The event can be booked, as the first event takes every time less than 20, but not including 20.

**Constraints:**

- `0 <= start < end <= 109`
- At most `1000` calls will be made to `book`.

#### Solution

##### Optimal Approach
[Video Explanation](https://youtu.be/45hKBSytI2E?t=754)

- **Set Data Structure**:
    
    - A set is chosen to store the events because it maintains sorted order based on the starting time of events. This allows efficient searching and insertion.
- **Booking an Event**:
    
    - The `book` function takes `start` and `end` times as input.
    - First, it uses `st.lower_bound({start, end})` to find the earliest event that starts **after** or at the same time as the event to be booked.
- **Checking Overlaps**:
    
    - **Next Event Overlap**: After finding the nearest event, it checks whether the new event overlaps with this next event by comparing the new event's end time (`end`) with the start time of the found event (`it->first`). If the next event starts before the new event ends, they overlap, and the booking fails.
    - **Previous Event Overlap**: It also checks if the new event overlaps with the previous event (if there is one). This is done by checking if the previous event's end time (`it->second`) is greater than the new event's start time (`start`). If so, the events overlap, and the booking fails.
- **Successful Booking**:
    
    - If no overlaps are found, the event is inserted into the set, and the function returns `true`.

*TC ->* O( log n ), for the book function
*SC ->* O( n )
```cpp title=Code
class MyCalendar {
    set<pair<int, int>> st;

public:
    MyCalendar() {}

    bool book(int start, int end) {

        // finding first element that starts just after or at same time as
        // {start,end}
        auto it = st.lower_bound({start, end});

        // check if current event overlaps with next event
        if (it != st.end() && it->first < end)
            return false;

        // check if current event overlaps with previous event
        if (it != st.begin()) {
            it--;
            if (start < it->second)
                return false;
        }

        st.insert({start, end});

        return true;
    }
};
```


##### Brute Force Approach

*TC ->* O( n ), for the book function
*SC ->* O( n )

```cpp title=Code
class MyCalendar {
    vector<pair<int, int>> v;

public:
    MyCalendar() {}

    bool book(int start, int end) {
        bool canBook = true;
        for (auto& [left, right] : v) {
            if (right <= start || end <= left)
                continue;
            else {
                canBook = false;
                break;
            }
        }

        if (canBook)
            v.push_back({start, end});

        return canBook;
    }
};
```

#### Similar Problems
- [[My Calendar II (LC731)]]
- [[My Calendar III (LC732)]]