---
difficulty: medium
leetcode-num: 731
topics:
  - Array
  - Ordered Set
  - Intervals
---
[Problem Link](https://leetcode.com/problems/my-calendar-ii/)

#### Problem
You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a **triple booking**.

A **triple booking** happens when three events have some non-empty intersection (i.e., some moment is common to all the three events.).

The event can be represented as a pair of integers `start` and `end` that represents a booking on the half-open interval `[start, end)`, the range of real numbers `x` such that `start <= x < end`.

Implement the `MyCalendarTwo` class:

- `MyCalendarTwo()` Initializes the calendar object.
- `boolean book(int start, int end)` Returns `true` if the event can be added to the calendar successfully without causing a **triple booking**. Otherwise, return `false` and do not add the event to the calendar.

**Example 1:**

**Input**
`["MyCalendarTwo", "book", "book", "book", "book", "book", "book"]`
`[[], [10, 20], [50, 60], [10, 40], [5, 15], [5, 10], [25, 55]]`
**Output**
`[null, true, true, true, false, true, true]`

**Explanation**
MyCalendarTwo myCalendarTwo = new MyCalendarTwo();
myCalendarTwo.book(10, 20); // return True, The event can be booked. 
myCalendarTwo.book(50, 60); // return True, The event can be booked. 
myCalendarTwo.book(10, 40); // return True, The event can be double booked. 
myCalendarTwo.book(5, 15);  // return False, The event cannot be booked, because it would result in a triple booking.
myCalendarTwo.book(5, 10); // return True, The event can be booked, as it does not use time 10 which is already double booked.
myCalendarTwo.book(25, 55); // return True, The event can be booked, as the time in `[25, 40)` will be double booked with the third event, the time [40, 50) will be single booked, and the time `[50, 55)` will be double booked with the second event.

**Constraints:**

- `0 <= start < end <= 109`
- At most `1000` calls will be made to `book`.

#### Solution

> [!NOTE] Optimal Approach
> The most optimal solution involves using two ordered sets (one for single booking and other for double bookings). I did not implement this because the constraints were low (at most 1000 bookings). See [[My Calendar I (LC729)#Optimal Approach]] for use of similar method.

##### Ordered Map Approach (Simpler)

This solution leverages a sweep line algorithm with a map to handle event bookings while avoiding triple overlaps. Here's the breakdown:
- **Map Setup:**  
    A map is used to track start and end times of events. A start time is marked with `+1`, indicating the beginning of an event, and an end time is marked with `-1`, indicating the conclusion of an event.
    
- **Booking Process:**  
    When a new event is booked, the corresponding start and end times are added to the map. This is done by incrementing the value at the start time and decrementing the value at the end time. These changes reflect a tentative booking.
    
- **Sweep Through the Map:**  
    The map is processed in time order (since it stores keys in sorted order). For each entry, the number of active bookings is updated by adding the event count. If, at any time, the number of active events reaches 3, it indicates that the booking will cause a triple overlap.
    
- **Handling Overlaps:**  
    If a triple overlap is detected, the tentative booking is undone. This is achieved by reversing the changes made to the map (i.e., removing the added start and end times), and the booking is rejected.
    
- **Successful Booking:**  
    If the entire process finishes without encountering a triple overlap, the event is confirmed as successfully booked.

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class MyCalendarTwo {
    map<int, int> mp; // key: time | val: +1 if start, -1 if end
public:
    MyCalendarTwo() {}

    bool book(int start, int end) {
        mp[start]++;
        mp[end]--;

        int booked = 0;

        for (auto& [key, val] : mp) {
            booked += val;

            // revert booking if we find 3 overlapping bookings
            if (booked == 3) {
                mp[start]--;
                mp[end]++;
                return false;
            }
        }
        return true;
    }
};
```

##### Two Arrays Approach
[Video Explanation](https://youtu.be/vySZ_KfjiRU)

###### Key Concepts:

1. **bookedRegions**: A vector that stores all single bookings (start and end times).
2. **doubleOverlapped**: A vector that keeps track of all regions where double bookings (two overlapping events) occur.
3. **checkOverlap**: A helper function to check if two intervals overlap.
4. **findOverlap**: A helper function that returns the overlapping region (if any) between two intervals.

###### Steps in the `book` function:

1. **Triple Booking Check**:
    
    - Before a new booking is allowed, it checks if adding the new event will create a triple booking.
    - This is done by iterating through all intervals in `doubleOverlapped`. If the new event overlaps with any region in `doubleOverlapped`, it means adding this event would create a triple booking, so the function returns `false` immediately to reject the booking.
2. **Double Booking Check**:
    
    - If the new event does not cause a triple booking, the function checks if it will cause a double booking by iterating through all intervals in `bookedRegions`.
    - For each overlap found, the overlapping portion is calculated using `findOverlap` and added to `doubleOverlapped`. This keeps track of areas where two events overlap.
3. **Booking the Event**:
    
    - After ensuring the event does not create a triple booking, it is added to `bookedRegions`, allowing the event to be officially booked.
    - The function returns `true` to indicate that the booking was successful.

###### Helper Functions:

- **checkOverlap(s1, e1, s2, e2)**:
    
    - Checks if two intervals `[s1, e1]` and `[s2, e2]` overlap. It does this by ensuring that the maximum of the start times is less than the minimum of the end times.
- **findOverlap(s1, e1, s2, e2)**:
    
    - Returns the overlapping portion of two intervals. The start of the overlap is the maximum of the two start times, and the end is the minimum of the two end times.

###### Why This Works:

- The solution allows for two events to overlap but ensures that no region is booked more than twice by tracking both single bookings and double bookings separately.
- Each time a booking is made, it first checks if the event causes a triple overlap by comparing it against the already double-booked regions.

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class MyCalendarTwo {
    typedef pair<int, int> P;
    vector<P> doubleOverlapped;
    vector<P> bookedRegions;

    bool checkOverlap(int s1, int e1, int s2, int e2) {
        return max(s1, s2) < min(e1, e2);
    }

    P findOverlap(int s1, int e1, int s2, int e2) {
        return {max(s1, s2), min(e1, e2)};
    }

public:
    MyCalendarTwo() {}

    bool book(int start, int end) {
        // check if triple booking created or not
        for (P region : doubleOverlapped) {
            if (checkOverlap(region.first, region.second, start, end))
                return false;
        }

        // check if double booking created.. if yes then add to doubleOverlapped
        for (P booking : bookedRegions) {
            if (checkOverlap(booking.first, booking.second, start, end)) {
                doubleOverlapped.push_back(
                    findOverlap(booking.first, booking.second, start, end));
            }
        }
        bookedRegions.push_back({start, end});
        return true;
    }
};
```

#### Similar Problems
- [[My Calendar I (LC729)]]
- [[My Calendar III (LC732)]]