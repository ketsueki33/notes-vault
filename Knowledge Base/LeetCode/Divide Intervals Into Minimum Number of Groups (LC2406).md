---
difficulty: medium
leetcode-num: 2406
topics:
  - Array
  - Intervals
  - Ordered Set
  - Greedy
  - Sorting
  - Heap
---
[Problem Link](https://leetcode.com/problems/divide-intervals-into-minimum-number-of-groups/)

#### Problem
You are given a 2D integer array `intervals` where `intervals[i] = [lefti, righti]` represents the **inclusive** interval `[lefti, righti]`.

You have to divide the intervals into one or more **groups** such that each interval is in **exactly** one group, and no two intervals that are in the same group **intersect** each other.

Return _the **minimum** number of groups you need to make_.

Two intervals **intersect** if there is at least one common number between them. For example, the intervals `[1, 5]` and `[5, 8]` intersect.

**Example 1:**

**Input:** intervals = `[[5,10],[6,8],[1,5],[2,3],[1,10]]`
**Output:** 3
**Explanation:** We can divide the intervals into the following groups:
- `Group 1: [1, 5], [6, 8].`
- `Group 2: [2, 3], [5, 10].`
- `Group 3: [1, 10].`
It can be proven that it is not possible to divide the intervals into fewer than 3 groups.

**Example 2:**

**Input:** `intervals = [[1,3],[5,6],[8,10],[11,13]]`
**Output:** 1
**Explanation:** None of the intervals overlap, so we can put all of them in one group.

**Constraints:**

- `1 <= intervals.length <= 105`
- `intervals[i].length == 2`
- `1 <= lefti <= righti <= 106`

#### Solution
##### Optimal Approach (Priority Queue)
[Video Explanation](https://youtu.be/aelafJoNaD0)

###### Key Concepts:

> [!note] 
> Think of each end time in the priority queue as if it is representing the end time for a whole group. The group that it's a part of.


1. **Priority Queue (Min-Heap)**:
    
    - The priority queue (`pq`) is used to keep track of the end times of the intervals. Since it's a min-heap, the smallest end time is always at the top.
    - This allows us to efficiently check when the next interval can be placed in the same group as a previous interval (i.e., when it does not overlap).
2. **Sort the intervals by start time**:
    
    - The intervals are first sorted by their start time. Sorting ensures that when processing intervals, you always know that later intervals have a start time greater than or equal to the current interval, helping with the decision of whether intervals overlap or not.
3. **Greedy approach**:
    
    - For each interval, check the earliest end time from the priority queue:
        - If the smallest end time (i.e., `pq.top()`) is **less than the start time** of the current interval, it means that the interval corresponding to the smallest end time has finished, and the current interval can use the same "group" (i.e., doesn't overlap).
        - If the intervals do overlap (i.e., `pq.top()` is greater than or equal to the start time), you need to add this interval to a new group.
    - After processing each interval, always push the current interval's end time to the priority queue.
4. **Size of the priority queue = minimum groups**:
    
    - The size of the priority queue at the end represents the minimum number of groups required. This is because each entry in the priority queue corresponds to an active group that has ongoing intervals that cannot be merged with other intervals.

###### Steps in the Algorithm:

1. **Sort the intervals**:  
    Sort all intervals based on their start time to ensure that when you process them, you're doing so in the order of increasing start times.
    
2. **Process each interval**:
    
    - For each interval, check if the interval with the earliest end time (top of the priority queue) has already finished by comparing its end time with the current interval's start time.
    - If the earliest interval has finished (`pq.top() < start`), pop it from the priority queue (remove it from the active group).
    - Push the current interval's end time into the priority queue to represent this interval being placed in a group.
3. **Return the number of groups**:
    
    - The size of the priority queue at the end represents the number of groups needed since each entry corresponds to an active group.

*TC ->* O( `n log n` )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    int minGroups(vector<vector<int>>& intervals) {
        priority_queue<int, vector<int>, greater<int>> pq;

        sort(intervals.begin(),intervals.end());

        for (const auto& i : intervals) {
            int start = i[0];
            int end = i[1];

            if (!pq.empty() && pq.top() < start)
                pq.pop();
            pq.push(end);
        }
        return pq.size();
    }
};
```

##### Slightly Less Optimal Approach (Ordered Map)
###### Key Concepts:

1. **Map to track changes in overlap**:
    
    - A `map<int, int>` is used to store when an interval starts and ends.
    - The key represents a point on the number line, and the value represents whether an interval starts or ends at that point:
        - `+1` when an interval starts.
        - `-1` when an interval ends (with `end + 1` to handle inclusivity).
2. **Why `mp[end + 1]--`?**:
    
    - Since the intervals are inclusive, an interval ending at a point should still overlap with another interval starting at that same point (e.g., `[3,5]` and `[5,7]` overlap at point `5`).
    - To prevent double-counting overlaps at the endpoint, the end time is incremented by 1 (`mp[end + 1]--`), effectively ending the interval after the point.
3. **Iterating over the map**:
    
    - The map is iterated to simulate sweeping a line across the intervals. As we process each key in the map:
        - We adjust the `consecutive` count based on whether intervals are starting or ending.
        - We keep track of the **maximum overlap** at any point using `maxConsecutive`, which represents the minimum number of groups required.

###### Steps in the Algorithm:

1. **Track interval boundaries**:  
    For each interval, increment the count at the start and decrement at the end (adjusted with `end + 1`).
2. **Sweep the number line**:  
    As you iterate through the map (which is sorted by default in increasing order), keep adding the values at each point to track how many intervals overlap at that point.
3. **Find the maximum overlap**:  
    Track the maximum overlap (`maxConsecutive`) to determine how many groups you need to accommodate all overlapping intervals.

###### Why this approach works:

- **Greedy approach**: By always tracking the maximum number of overlapping intervals at any point, you ensure that you can group intervals in the fewest number of sets.
- **Efficiency**: Using a map allows the algorithm to efficiently count the number of active intervals at any given time without sorting the intervals directly.

*TC ->* O( `n log n` )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    int minGroups(vector<vector<int>>& intervals) {
        map<int, int> mp;
        int consecutive = 0, maxConsecutive = 0;

        for (vector<int>& v : intervals) {
            int start = v[0];
            int end = v[1];

            mp[start]++;

            // +1 coz interval is inclusive (eg. [3,5],[5,7] should overlap)
            mp[end + 1]--;
        }

        for (auto& [key, val] : mp) {
            consecutive += val;

            maxConsecutive = max(maxConsecutive, consecutive);
        }

        return maxConsecutive;
    }
};
```

