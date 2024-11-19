---
difficulty: hard
leetcode-num: 732
topics:
  - Array
  - Ordered Set
  - Intervals
---
[Problem Link](https://leetcode.com/problems/my-calendar-iii/)
#### Problem
A `k`-booking happens when `k` events have some non-empty intersection (i.e., there is some time that is common to all `k` events.)

You are given some events `[startTime, endTime)`, after each given event, return an integer `k` representing the maximum `k`-booking between all the previous events.

Implement the `MyCalendarThree` class:

- `MyCalendarThree()` Initializes the object.
- `int book(int startTime, int endTime)` Returns an integer `k` representing the largest integer such that there exists a `k`-booking in the calendar.

**Example 1:**

**Input**
`["MyCalendarThree", "book", "book", "book", "book", "book", "book"]`
`[[], [10, 20], [50, 60], [10, 40], [5, 15], [5, 10], [25, 55]]`
**Output**
`[null, 1, 1, 2, 3, 3, 3]`

**Explanation**
MyCalendarThree myCalendarThree = new MyCalendarThree();
myCalendarThree.book(10, 20); // return 1
myCalendarThree.book(50, 60); // return 1
myCalendarThree.book(10, 40); // return 2
myCalendarThree.book(5, 15); // return 3
myCalendarThree.book(5, 10); // return 3
myCalendarThree.book(25, 55); // return 3

**Constraints:**

- `0 <= startTime < endTime <= 109`
- At most `400` calls will be made to `book`.

#### Solution
##### Ordered Map Approach
We will just use a map to count the number of bookings active at any time.

- **Map Setup:**
    
    - A `map<int, int>` stores events, where the keys are time points (start or end times), and the values represent how the booking affects the count of active events at that time.
    - `+1` indicates that an event starts, increasing the count of active bookings.
    - `-1` indicates that an event ends, decreasing the count of active bookings.
- **Booking Process:**
    
    - Each time a new booking is made, the `start` and `end` times are updated in the map. The start time gets a `+1`, and the end time gets a `-1`, indicating the respective change in active bookings.
- **Calculating Overlaps:**
    
    - The map is iterated over in time order (because `map` keeps keys sorted).
    - For each time point, the number of active bookings (`currBooked`) is updated by adding the value at that time.
    - While iterating, the solution also tracks the maximum number of concurrent bookings (`maxBooked`). This is done by comparing the current number of active bookings to the previously recorded maximum.
- **Result:**
    
    - After processing all time points, the solution returns `maxBooked`, which represents the maximum number of overlapping events at any point.

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class MyCalendarThree {
    map<int, int> mp; // key: time | val: +1 if start, -1 if end

public:
    MyCalendarThree() {}

    int book(int start, int end) {
        mp[start]++;
        mp[end]--;

        int currBooked = 0;
        int maxBooked = 0;

        for (auto& [key, val] : mp) {
            currBooked += val;
            maxBooked = max(maxBooked, currBooked);
        }
        return maxBooked;
    }
};
```

#### Similar Problems
- [[My Calendar I (LC729)]]
- [[My Calendar II (LC731)]]
