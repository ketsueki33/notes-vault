---
difficulty: hard
leetcode-num: 2463
topics:
  - Array
  - Dynamic Programming
  - Sorting
---
[Problem Link](https://leetcode.com/problems/minimum-total-distance-traveled/)

#### Problem
There are some robots and factories on the X-axis. You are given an integer array `robot` where `robot[i]` is the position of the `ith` robot. You are also given a 2D integer array `factory` where `factory[j] = [positionj, limitj]` indicates that `positionj` is the position of the `jth` factory and that the `jth` factory can repair at most `limitj` robots.

The positions of each robot are **unique**. The positions of each factory are also **unique**. Note that a robot can be **in the same position** as a factory initially.

All the robots are initially broken; they keep moving in one direction. The direction could be the negative or the positive direction of the X-axis. When a robot reaches a factory that did not reach its limit, the factory repairs the robot, and it stops moving.

**At any moment**, you can set the initial direction of moving for **some** robot. Your target is to minimize the total distance traveled by all the robots.

Return _the minimum total distance traveled by all the robots_. The test cases are generated such that all the robots can be repaired.

**Note that**

- All robots move at the same speed.
- If two robots move in the same direction, they will never collide.
- If two robots move in opposite directions and they meet at some point, they do not collide. They cross each other.
- If a robot passes by a factory that reached its limits, it crosses it as if it does not exist.
- If the robot moved from a position `x` to a position `y`, the distance it moved is `|y - x|`.

**Example 1:**

![|300](https://assets.leetcode.com/uploads/2022/09/15/example1.jpg)

**Input:** `robot = [0,4,6], factory = [[2,2],[6,2]]`
**Output:** 4
**Explanation:** As shown in the figure:
- The first robot at position 0 moves in the positive direction. It will be repaired at the first factory.
- The second robot at position 4 moves in the negative direction. It will be repaired at the first factory.
- The third robot at position 6 will be repaired at the second factory. It does not need to move.
The limit of the first factory is 2, and it fixed 2 robots.
The limit of the second factory is 2, and it fixed 1 robot.
The total distance is |2 - 0| + |2 - 4| + |6 - 6| = 4. It can be shown that we cannot achieve a better total distance than 4.

**Example 2:**

![|300](https://assets.leetcode.com/uploads/2022/09/15/example-2.jpg)

**Input:** `robot = [1,-1], factory = [[-2,1],[2,1]]`
**Output:** 2
**Explanation:** As shown in the figure:
- The first robot at position 1 moves in the positive direction. It will be repaired at the second factory.
- The second robot at position -1 moves in the negative direction. It will be repaired at the first factory.
The limit of the first factory is 1, and it fixed 1 robot.
The limit of the second factory is 1, and it fixed 1 robot.
The total distance is |2 - 1| + |(-2) - (-1)| = 2. It can be shown that we cannot achieve a better total distance than 2.

**Constraints:**

- `1 <= robot.length, factory.length <= 100`
- `factory[j].length == 2`
- `-109 <= robot[i], positionj <= 109`
- `0 <= limitj <= robot.length`
- The input will be generated such that it is always possible to repair every robot.

#### Solution
[Video Explanation](https://youtu.be/_wxgR1qMvFE)

##### Optimal Approach

###### Key Concepts

1. **Dynamic Programming (DP)**: The `dp` table stores previously computed results to avoid redundant calculations.
2. **Recursive Solution with Choices**: For each robot and factory, we decide whether to repair the robot at the current factory or skip to another factory, aiming to minimize the distance.
3. **Factory Capacity**: Each factory has a capacity (number of robots it can repair), which is flattened into individual factory positions in the `factories` vector.

###### Steps in Solution

1. **Initialize Variables and Data Structures**

- Define constants and types (`MAX` as a large value and `ll` as `long long`).
- `dp`: A 2D vector to store the minimum distance for each combination of robots (`r`) and factories (`f`).
- `m` and `n`: Sizes of `robot` and `factories`, respectively.

---
2. **Flatten Factory Capacities**

- **Sort** the `robot` array by position to simplify the problem.
- **Flatten** each factory's capacity into individual factory positions. For example, if a factory at position `5` can repair `3` robots, add three `5`s to `factories`.
- This enables direct one-to-one assignments with `factories` for a DP solution.
---

3. **Recursive DP Function (`solve`)**

- **Base Cases**:
    - If `r < 0`, return `0`, as all robots have been repaired.
    - If `f < 0` but `r >= 0`, return `MAX`, as no factories are left but robots still need repairs.
- **Memoization**: If `dp[r][f]` is already calculated, return its value.
- **Choice 1** - `repair`: Repair the current robot `r` at factory `f`, and calculate the cost as:
    - `repair = abs(robot[r] - factories[f]) + solve(robot, factories, r - 1, f - 1)`.
- **Choice 2** - `noRepair`: Skip this factory and calculate the cost to repair `r` at a later factory:
    - `noRepair = solve(robot, factories, r, f - 1)`.
- **Memoize and Return**: Store `dp[r][f] = min(repair, noRepair)` and return it.
---

4. **Solve the Problem with Initial Call**

- After setting up `dp`, initiate the solution with `solve(robot, factories, m - 1, n - 1)`, which attempts to repair all robots starting from the last robot and last factory.


> [!info] Why sorting?
> - The solution relies on a "greedy" principle that if we assign factories to robots in order of their proximity (smallest distances first), we get the smallest cumulative distance.
> - Sorting ensures that we evaluate each robot starting from the nearest available factory.
> - the solution assumes **no criss-crossing** in assignments between robots and factories.
> - By sorting both `robots` and `factories`, we create a natural order where each robot is "encouraged" to match with a nearby factory. This approach prevents any robot from jumping over another robot’s closest factory, which would cause criss-crossing.


*TC ->* O( `m * n` )
*SC ->* O(`m * n`  )

```cpp title=Code
#define MAX 10e12
using ll = long long;

class Solution {
    vector<vector<ll>> dp;
    int m, n;

    long long solve(vector<int>& robot, vector<int>& factories, int r, int f) {
        if (r < 0)
            return 0; // all robots repaired
        if (f < 0)
            return MAX; // no factories left and robots still left

        if (dp[r][f] != -1)
            return dp[r][f];

        ll repair = abs(robot[r] - factories[f]) +
                    solve(robot, factories, r - 1, f - 1);

        ll noRepair = solve(robot, factories, r, f - 1);

        return dp[r][f] = min(repair, noRepair);
    }

public:
    long long minimumTotalDistance(vector<int>& robot,
                                   vector<vector<int>>& factory) {
        sort(robot.begin(), robot.end());
        sort(factory.begin(), factory.end());

        vector<int> factories;
        for (auto x : factory) {
            int pos = x[0];
            int count = x[1];

            while (count--)
                factories.push_back(pos);
        }

        m = robot.size();
        n = factories.size();
        dp.resize(m, vector<ll>(n, -1));

        return solve(robot, factories, m - 1, n - 1);
    }
};
```