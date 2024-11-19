---
difficulty: medium
leetcode-num: 1514
topics:
  - Graph
  - Shortest Path
  - Heap
---
[Problem Link]()

#### Problem
You are given an undirected weighted graph of `n` nodes (0-indexed), represented by an edge list where `edges[i] = [a, b]` is an undirected edge connecting the nodes `a` and `b` with a probability of success of traversing that edge `succProb[i]`.

Given two nodes `start` and `end`, find the path with the maximum probability of success to go from `start` to `end` and return its success probability.

If there is no path from `start` to `end`, **return 0**. Your answer will be accepted if it differs from the correct answer by at most **1e-5**.

**Example 1:**

**![|200](https://assets.leetcode.com/uploads/2019/09/20/1558_ex1.png)**

**Input:** n = 3, edges = `[[0,1],[1,2],[0,2]]`, succProb = `[0.5,0.5,0.2]`, start = 0, end = 2
**Output:** 0.25000
**Explanation:** There are two paths from start to end, one having a probability of success = 0.2 and the other has 0.5 * 0.5 = 0.25.

**Example 2:**

**![|200](https://assets.leetcode.com/uploads/2019/09/20/1558_ex2.png)**

**Input:** n = 3, edges = `[[0,1],[1,2],[0,2]]`, succProb = `[0.5,0.5,0.3]`, start = 0, end = 2
**Output:** 0.30000

**Example 3:**

**![|200](https://assets.leetcode.com/uploads/2019/09/20/1558_ex3.png)**

**Input:** n = 3, edges = `[[0,1]]`, succProb = `[0.5]`, start = 0, end = 2
**Output:** 0.00000
**Explanation:** There is no path between 0 and 2.

**Constraints:**

- `2 <= n <= 10^4`
- `0 <= start, end < n`
- `start != end`
- `0 <= a, b < n`
- `a != b`
- `0 <= succProb.length == edges.length <= 2*10^4`
- `0 <= succProb[i] <= 1`
- There is at most one edge between every two nodes.

#### Solution
[Video Explanation](https://youtu.be/zTM9k6jqpXI)

##### Optimal Approach
We will be using Djikstra's Algorithms with a slight variations: 
- We will do multiplication instead of summation of edge weights. 
- We we will be calculating largest probability instead of shortest path. (maximum instead of minimum)
- Max Heap instead of Min Heap

*TC ->* O( E log(v) )
*SC ->* O(  )

```cpp title=Code
class Solution {
    typedef pair<double, int> P;

public:
    double maxProbability(int n, vector<vector<int>>& edges,
                          vector<double>& succProb, int start, int end) {
        unordered_map<int, vector<pair<int, double>>> adj;
        for (int i = 0; i < edges.size(); i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            double prob = succProb[i];

            adj[u].push_back({v, prob});
            adj[v].push_back({u, prob});
        }

        vector<double> result(n, 0);

        priority_queue<P> pq;

        result[start] = 1; // probability to reach start from start is 100%
        pq.push({1.0, start});

        while (!pq.empty()) {
            double currProb = pq.top().first;
            int currNode = pq.top().second;
            pq.pop();

            for (auto& temp : adj[currNode]) {
                double adjProb = temp.second;
                int adjNode = temp.first;

                if (result[adjNode] < adjProb * currProb) {
                    result[adjNode] = adjProb * currProb;
                    pq.push({result[adjNode], adjNode});
                }
            }
        }
        return result[end];
    }
};
```