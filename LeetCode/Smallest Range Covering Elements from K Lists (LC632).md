---
difficulty: hard
leetcode-num: 632
topics:
  - Array
  - Heap
  - Sliding Window
  - Greedy
---
[Problem Link](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/)

#### Problem
You have `k` lists of sorted integers in **non-decreasing order**. Find the **smallest** range that includes at least one number from each of the `k` lists.

We define the range `[a, b]` is smaller than range `[c, d]` if `b - a < d - c` **or** `a < c` if `b - a == d - c`.

**Example 1:**

**Input:** `nums = [[4,10,15,24,26],[0,9,12,20],[5,18,22,30]]`
**Output:** `[20,24]`
**Explanation:** 
`List 1: [4, 10, 15, 24,26], 24 is in range [20,24].`
`List 2: [0, 9, 12, 20], 20 is in range [20,24].`
`List 3: [5, 18, 22, 30], 22 is in range [20,24].`

**Example 2:**

**Input:** `nums = [[1,2,3],[1,2,3],[1,2,3]]`
**Output:** `[1,1]`

**Constraints:**

- `nums.length == k`
- `1 <= k <= 3500`
- `1 <= nums[i].length <= 50`
- `-105 <= nums[i][j] <= 105`
- `nums[i]` is sorted in **non-decreasing** order.

#### Solution
##### Optimal Approach (Priority Queue)
[Video Explanation](https://youtu.be/0IqFMBatlhU)


> [!info] 
> This approach builds upon the [[#Better Approach]] by using a priority queue instead of a pointers array to store the values from each group. This makes finding the minimum element more efficient.

###### Key Concepts:

1. **Priority Queue (Min-Heap)**:
    
    - The priority queue stores the smallest element from each array at any given point. Each element in the queue is represented as a triplet: `{value, list index, index within the list}`.
    - The queue is structured to always give us the smallest element (`minE`) across all the arrays, which helps in tracking the range.
2. **Tracking the Maximum Element (`maxE`)**:
    
    - This approach also keeps track of the maximum element (`maxE`) currently in the window. This ensures that the range can be dynamically updated without scanning all elements every time.
    - By knowing both the smallest (`minE`) and largest (`maxE`) elements at any point, we can efficiently compute the current range and update it if necessary.
3. **Expanding the Range**:
    
    - Initially, the first element from each array is inserted into the priority queue, and `maxE` is initialized to track the largest of these first elements.
    - As we extract the smallest element from the priority queue (`minE`), we add the next element from the same array to the queue and update `maxE` if necessary.
    - This process ensures that the range always includes at least one element from each array, while dynamically adjusting the window.

###### Detailed Breakdown:

1. **Initializing the Priority Queue**:
    
    - For each array, we insert the first element into the priority queue. This guarantees that the queue holds one element from each array.
    - We simultaneously initialize `maxE` to the largest of these first elements. This helps us track the maximum element in the current window.
2. **Processing Elements in the Priority Queue**:
    
    - The smallest element (`minE`) is extracted from the priority queue, which is always the top element.
    - We then check if the current range `[minE, maxE]` is smaller than the previously found range. If it is, we update the result with this new range.
3. **Inserting the Next Element**:
    
    - After extracting `minE`, we add the next element from the same array to the priority queue (if it exists). This ensures that we continue to have one element from each array in the window.
    - If the newly added element is larger than the current `maxE`, we update `maxE`. This ensures that `maxE` always reflects the largest element in the current window.
4. **Termination**:
    
    - The process continues until we can no longer add elements from one of the arrays, meaning the current range can't be extended further.
    - At this point, the smallest valid range found is returned as the result.

_TC ->_ O( `n * log k` )  
_SC ->_ O( k )

```cpp title=Code
class Solution {

public:
    vector<int> smallestRange(vector<vector<int>>& nums) {
        int k = nums.size();
        vector<int> res = {static_cast<int>(-1e5), static_cast<int>(1e5)};

        // each element in pq will store {value, List Index, Index of value in
        // it's List}
        priority_queue<vector<int>, vector<vector<int>>, greater<vector<int>>>
            pq;

        int maxE = INT_MIN;
        for (int i = 0; i < k; i++) {
            int ele = nums[i][0];

            maxE = max(maxE, ele);
            pq.push({ele, i, 0});
        }

        while (!pq.empty()) {
            vector<int> curr = pq.top();
            pq.pop();

            int minE = curr[0];
            int minElListIdx = curr[1];
            int idx = curr[2];

            if (maxE - minE < res[1] - res[0])
                res = {minE, maxE};

            int nextIndex = idx + 1;

            if (nextIndex >= nums[minElListIdx].size())
                break;

            int nextEle = nums[minElListIdx][nextIndex];

            pq.push({nextEle, minElListIdx, nextIndex});
            maxE = max(maxE, nextEle);
        }
        return res;
    }
};
```

##### Slightly Less Optimal Approach (Sliding Window)
###### Key Concepts:

1. **Priority Queue (Min-Heap)**: The priority queue is used to keep track of the smallest element across all arrays. It stores the current smallest element along with its corresponding array index and its position within that array.
    
2. **Tracking the Current Range**: The solution builds the smallest range by maintaining the smallest (`minE`) and largest (`maxE`) elements currently being considered from the arrays. As we process elements using the priority queue, we dynamically adjust this range.
    
3. **Expanding and Contracting the Range**: As we extract elements from the priority queue, we add the next element from the same array to the queue. This ensures that the range always covers one element from each array. By keeping track of the largest element encountered, we maintain a range that includes elements from all arrays.
    

###### Detailed Breakdown:

1. **Initializing the Priority Queue**:
    
    - Each array's first element is inserted into the priority queue. The queue stores each element as a triplet: `{value, array index, element index}`.
    - The smallest element (from any array) is always at the top of the priority queue.
2. **Building a Sorted List (`v`)**:
    
    - The priority queue is used to construct a sorted list of elements (`v`), where each entry contains both the element value and the index of the array it came from.
    - For each element extracted from the priority queue, the next element from the same array is inserted back into the queue. This keeps the process moving through all arrays while maintaining the smallest element at the top of the queue.
3. **Finding the Smallest Range**:
    
    - Once the sorted list is built, a sliding window approach is used to find the smallest range that covers at least one element from each array.
    - The `tracker` keeps a count of how many elements from each array are included in the current window. We expand the window by increasing `high` and shrink it by moving `low` when we have included at least one element from every array.
4. **Updating the Result**:
    
    - Whenever we find a valid range (i.e., the window includes at least one element from every array), we check if it is smaller than the previously found best range. If so, we update the result with the new range.
5. **Termination**:
    
    - The process ends once we've processed all elements from the sorted list (`v`). The smallest valid range found is returned as the result.

*TC ->* O( `n * log k` )
*SC ->* O( n )

```cpp title=Code
class Solution {

public:
    vector<int> smallestRange(vector<vector<int>>& nums) {
        int k = nums.size();
        vector<int> res = {static_cast<int>(-1e5), static_cast<int>(1e5)};

        priority_queue<vector<int>, vector<vector<int>>, greater<vector<int>>>
            pq;

        for (int i = 0; i < k; i++)
            pq.push({nums[i][0], i, 0});

        vector<pair<int, int>> v;
        while (!pq.empty()) {
            auto curr = pq.top();
            pq.pop();

            int val = curr[0];
            int group = curr[1];
            int idx = curr[2];

            v.push_back({val, group});

            if (idx + 1 < nums[group].size()) {
                pq.push({nums[group][idx + 1], group, idx + 1});
            }
        }

        int high = -1;
        unordered_map<int, int> tracker;

        for (int low = 0; low < v.size(); low++) {
            while (tracker.size() != k) {
                if (++high >= v.size())
                    break;
                tracker[v[high].second]++;
            }

            if (tracker.size() != k)
                break;

            if (v[high].first - v[low].first < res[1] - res[0])
                res = {v[low].first, v[high].first};

            tracker[v[low].second]--;
            if (tracker[v[low].second] == 0)
                tracker.erase(v[low].second);
        }

        return res;
    }
};
```

##### Better Approach
[Video Explanation](https://youtu.be/0IqFMBatlhU)
###### Key Concepts:

1. **Multiple Pointers**: Each sorted array has a pointer (index) that tracks the current element being considered. Initially, each pointer starts at the first element of its corresponding array.
    
2. **Min-Max Tracking**: In each iteration, the solution finds the minimum and maximum values across the elements pointed to by the current pointers of all arrays. This ensures that the current range includes at least one number from each array.
    
3. **Shrinking the Range**: The algorithm always tries to minimize the range by:
    
    - Calculating the current range as the difference between the maximum and minimum elements found.
    - If the new range is smaller than the previously found range, it updates the result.
4. **Moving the Pointer**: After evaluating a range, the pointer of the array that contains the minimum element is moved one step forward. This is because to minimize the range further, we need to increase the minimum element.
    
5. **Termination Condition**: The process repeats until the pointer of the array with the minimum element reaches the end of that array. At this point, it's no longer possible to form a valid range that includes at least one element from every array.
    

###### Detailed Breakdown:

1. **minMax Function**:
    
    - This function scans through all `k` arrays to find the minimum and maximum elements pointed to by the current pointers.
    - It also records the index of the array containing the minimum and maximum elements.
    
    This helps track both the current range (`minE`, `maxE`) and the array (`minI`) whose pointer needs to be moved forward next.
    
2. **Main Logic (smallestRange)**:
    
    - **Initialization**:
        - `pointers`: Keeps track of the current position in each array. Initially, all pointers are at the start (`0`).
        - `res`: Stores the best (smallest) range found so far. It is initialized to a large range (`[-1e5, 1e5]`).
    - **While Loop**:
        - In each iteration, it calls `minMax` to find the current minimum and maximum values from the pointers.
        - It checks if the current range is smaller than the best range (`res`) found so far. If so, it updates `res`.
        - It moves the pointer of the array with the minimum value forward. This step helps narrow the range.
        - The loop continues until one of the pointers reaches the end of its array, meaning it's no longer possible to form a valid range.
3. **Final Result**: The function returns `res`, which contains the smallest range that includes at least one number from each array.


*TC ->* O( `n * k` )
*SC ->* O( k )

```cpp title=Code
class Solution {
    int k;
    vector<int> minMax(vector<int>& pointers, vector<vector<int>>& nums) {
        int maxE = INT_MIN, minE = INT_MAX, maxI, minI;

        for (int i = 0; i < k; i++) {
            int ele = nums[i][pointers[i]];
            if (ele < minE) {
                minE = ele;
                minI = i;
            }
            if (ele > maxE) {
                maxE = ele;
                maxI = i;
            }
        }
        return {maxE, maxI, minE, minI};
    }

public:
    vector<int> smallestRange(vector<vector<int>>& nums) {
        k = nums.size();
        vector<int> res = {static_cast<int>(-1e5), static_cast<int>(1e5)};
        vector<int> pointers(k, 0);

        while (true) {
            vector<int> temp = minMax(pointers, nums);
            int maxE = temp[0];
            int minE = temp[2];
            int minElListIdx = temp[3];

            if (maxE - minE < res[1] - res[0])
                res = {minE, maxE};

            int nextIndex = pointers[minElListIdx] + 1;

            if (nextIndex >= nums[minElListIdx].size())
                break;

            pointers[minElListIdx] = nextIndex;
        }
        return res;
    }
};
```

##### Brute Force Approach
We will put all the numbers in a single sorted array and then try for every possible range. For each range, we will check if it contains number from each group.. and update result accordingly.