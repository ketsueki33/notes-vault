---
difficulty: medium
leetcode-num: 1942
topics:
  - Array
  - Heap
  - Intervals
---
[Problem Link](https://leetcode.com/problems/the-number-of-the-smallest-unoccupied-chair/)

#### Problem
There is a party where `n` friends numbered from `0` to `n - 1` are attending. There is an **infinite** number of chairs in this party that are numbered from `0` to `infinity`. When a friend arrives at the party, they sit on the unoccupied chair with the **smallest number**.

- For example, if chairs `0`, `1`, and `5` are occupied when a friend comes, they will sit on chair number `2`.

When a friend leaves the party, their chair becomes unoccupied at the moment they leave. If another friend arrives at that same moment, they can sit in that chair.

You are given a **0-indexed** 2D integer array `times` where `times[i] = [arrivali, leavingi]`, indicating the arrival and leaving times of the `ith` friend respectively, and an integer `targetFriend`. All arrival times are **distinct**.

Return _the **chair number** that the friend numbered_ `targetFriend` _will sit on_.

**Example 1:**

**Input:** `times = [[1,4],[2,3],[4,6]], targetFriend = 1`
**Output:** 1
**Explanation:** 
- Friend 0 arrives at time 1 and sits on chair 0.
- Friend 1 arrives at time 2 and sits on chair 1.
- Friend 1 leaves at time 3 and chair 1 becomes empty.
- Friend 0 leaves at time 4 and chair 0 becomes empty.
- Friend 2 arrives at time 4 and sits on chair 0.
Since friend 1 sat on chair 1, we return 1.

**Example 2:**

**Input:** `times = [[3,10],[1,5],[2,6]], targetFriend = 0`
**Output:** 2
**Explanation:** 
- Friend 1 arrives at time 1 and sits on chair 0.
- Friend 2 arrives at time 2 and sits on chair 1.
- Friend 0 arrives at time 3 and sits on chair 2.
- Friend 1 leaves at time 5 and chair 0 becomes empty.
- Friend 2 leaves at time 6 and chair 1 becomes empty.
- Friend 0 leaves at time 10 and chair 2 becomes empty.
Since friend 0 sat on chair 2, we return 2.

**Constraints:**

- `n == times.length`
- `2 <= n <= 104`
- `times[i].length == 2`
- `1 <= arrivali < leavingi <= 105`
- `0 <= targetFriend <= n - 1`
- Each `arrivali` time is **distinct**.

#### Solution
##### Optimal Approach - Two Priority Queues
###### Key Concepts:

1. **Two priority queues**:
    
    - `freeChairs`: Min-heap that keeps track of the available chairs.
    - `busyChairs`: Min-heap that keeps track of chairs that are currently occupied, ordered by the time the friend will leave.
2. **Steps**:
    
    - **Initialize chairs**: All chairs are numbered from 0 to `n - 1`, where `n` is the number of friends. These are stored in the `freeChairs` heap.
    - **Sort by arrival time**: The `times` array is sorted by arrival time so that friends are processed in the order they arrive.

###### Walkthrough of the Solution:

1. **Initialize Priority Queues**:
    
    - A min-heap `freeChairs` holds the chair indices (0, 1, 2, ...). These chairs will be assigned to friends when they arrive.
    - A second min-heap `busyChairs` holds pairs of (departure time, chair index). This keeps track of when each friend will leave and release their chair.
2. **Process Each Friend**:
    
    - First, the solution sorts the `times` array by the arrival times of friends, ensuring we handle them in the order they arrive.
    - For each friend `i`, the algorithm checks if any friends currently occupying chairs have left. It does this by checking if the top element in `busyChairs` has a departure time earlier than or equal to the current friend's arrival time. If so, that chair is freed and added back to `freeChairs`.
3. **Assign a Chair**:
    
    - After releasing any chairs that have become free, the next available chair (from the top of `freeChairs`) is assigned to the current friend.
    - If the current friend's arrival time matches the target friend's arrival time, this chair is the one assigned to the target friend, and it is returned.
4. **Return the Chair for the Target Friend**:
    
    - Once the target friend's arrival time is reached, the solution returns the chair assigned to them.
5. **Continue Processing**:
    
    - For other friends, the solution pushes a pair of (departure time, chair index) into `busyChairs` so that their chair will become free when they leave.

*TC ->* O( `n log n`  )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    int smallestChair(vector<vector<int>>& times, int targetFriend) {
        int targetArrival = times[targetFriend][0];

        priority_queue<int, vector<int>, greater<int>> freeChairs;
        priority_queue<pair<int, int>, vector<pair<int, int>>,
                       greater<pair<int, int>>>
            busyChairs;

        for (int i = 0; i < times.size(); i++)
            freeChairs.push(i);

        sort(times.begin(), times.end());

        for (int i = 0; i < times.size(); i++) {
            int arr = times[i][0];
            int dep = times[i][1];

            // put back empty chairs
            while (!busyChairs.empty() && busyChairs.top().first <= arr) {
                freeChairs.push(busyChairs.top().second);
                busyChairs.pop();
            }

            int chair = freeChairs.top();
            freeChairs.pop();

            if (arr == targetArrival)
                return chair;

            busyChairs.push({dep, chair});
        }

        return 0;
    }
};
```

##### Priority Queue and Map Approach
This was the first approach that came to mind.

It can be improved upon by using a second priority queue instead of a map to keep track of departure times.

*TC ->* O( `n log n`  )
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    int smallestChair(vector<vector<int>>& times, int targetFriend) {
        int targetArrival = times[targetFriend][0];
        priority_queue<int, vector<int>, greater<int>> pq;
        map<int, vector<int>> mp;

        for (int i = 0; i < times.size(); i++)
            pq.push(i);

        sort(times.begin(), times.end());

        for (int i = 0; i < times.size(); i++) {
            int arr = times[i][0];
            int dep = times[i][1];

            // put back empty chairs
            while (!mp.empty()) {
                auto it = mp.begin();

                if (it->first > arr)
                    break;
                for (int x : it->second)
                    pq.push(x);

                mp.erase(it);
            }

            int chair = pq.top();
            pq.pop();

            if (arr == targetArrival)
                return chair;

            mp[dep].push_back(chair);
        }

        return 0;
    }
};
```