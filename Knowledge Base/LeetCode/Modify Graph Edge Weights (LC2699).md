---
difficulty: hard
leetcode-num: 2499
topics:
  - Graph
  - Heap
  - Shortest Path
---
[Problem Link](https://leetcode.com/problems/modify-graph-edge-weights/)

#### Problem
You are given an **undirected weighted** **connected** graph containing `n` nodes labeled from `0` to `n - 1`, and an integer array `edges` where `edges[i] = [ai, bi, wi]` indicates that there is an edge between nodes `ai` and `bi` with weight `wi`.

Some edges have a weight of `-1` (`wi = -1`), while others have a **positive** weight (`wi > 0`).

Your task is to modify **all edges** with a weight of `-1` by assigning them **positive integer values** in the range `[1, 2 * 109]` so that the **shortest distance** between the nodes `source` and `destination` becomes equal to an integer `target`. If there are **multiple** **modifications** that make the shortest distance between `source` and `destination` equal to `target`, any of them will be considered correct.

Return _an array containing all edges (even unmodified ones) in any order if it is possible to make the shortest distance from_ `source` _to_ `destination` _equal to_ `target`_, or an **empty array** if it's impossible._

**Note:** You are not allowed to modify the weights of edges with initial positive weights.

**Example 1:**

**![|250](https://assets.leetcode.com/uploads/2023/04/18/graph.png)**

**Input:** n = 5, edges = `[[4,1,-1],[2,0,-1],[0,3,-1],[4,3,-1]]`, source = 0, destination = 1, target = 5
**Output:** `[[4,1,1],[2,0,1],[0,3,3],[4,3,1]]`
**Explanation:** The graph above shows a possible modification to the edges, making the distance from 0 to 1 equal to 5.

**Example 2:**

**![|250](https://assets.leetcode.com/uploads/2023/04/18/graph-2.png)**

**Input:** n = 3, edges = `[[0,1,-1],[0,2,5]]`, source = 0, destination = 2, target = 6
**Output:** []
**Explanation:** The graph above contains the initial edges. It is not possible to make the distance from 0 to 2 equal to 6 by modifying the edge with weight -1. So, an empty array is returned.

**Example 3:**

**![|250](https://assets.leetcode.com/uploads/2023/04/19/graph-3.png)**

**Input:** n = 4, edges = `[[1,0,4],[1,2,3],[2,3,5],[0,3,-1]]`, source = 0, destination = 2, target = 6
**Output:** `[[1,0,4],[1,2,3],[2,3,5],[0,3,1]]`
**Explanation:** The graph above shows a modified graph having the shortest distance from 0 to 2 as 6.

**Constraints:**

- `1 <= n <= 100`
- `1 <= edges.length <= n * (n - 1) / 2`
- `edges[i].length == 3`
- `0 <= ai, bi < n`
- `wi = -1` or `1 <= wi <= 107`
- `ai != bi`
- `0 <= source, destination < n`
- `source != destination`
- `1 <= target <= 109`
- The graph is connected, and there are no self-loops or repeated edges

#### Solution
[Video Explanation](https://youtu.be/F6sRIQKslwA)

##### Optimal Approach

*TC ->* O( E$^{2}$ log V )
*SC ->* O( V+E )

```cpp title=Code
class Solution {
    typedef long long ll;
    typedef pair<ll, ll> P;
    const int LARGE_VAL = 2e9;

    ll djikstra(int n, vector<vector<int>>& edges, int src, int dstn) {
        // make adjacency list excluding -1 edge weights
        unordered_map<ll, vector<P>> adj;

        for (auto& edge : edges) {
            ll u = edge[0];
            ll v = edge[1];
            int wt = edge[2];
            if (wt == -1)
                continue;
            adj[u].push_back({v, wt});
            adj[v].push_back({u, wt});
        }

        priority_queue<P, vector<P>, greater<P>> pq;
        vector<ll> result(n, INT_MAX);
        vector<bool> visited(n, false);

        result[src] = 0;
        pq.push({0, src});

        while (!pq.empty()) {
            ll currDist = pq.top().first;
            ll currNode = pq.top().second;
            pq.pop();

            if (visited[currNode])
                continue;

            visited[currNode] = true;

            for (auto& [nbr, wt] : adj[currNode]) {
                ll newDist = currDist + wt;
                if (newDist < result[nbr]) {
                    result[nbr] = newDist;
                    pq.push({newDist, nbr});
                }
            }
        }
        return result[dstn];
    }

public:
    vector<vector<int>> modifiedGraphEdges(int n, vector<vector<int>>& edges,
                                           int src, int dstn, int target) {
        ll currShortestDist = djikstra(n, edges, src, dstn);

        if (currShortestDist < target)
            return {};

        bool matchedTarget = (currShortestDist == target);

        for (vector<int>& edge : edges) {
            if (edge[2] != -1)
                continue;

            edge[2] = matchedTarget ? LARGE_VAL : 1;

            if (matchedTarget != true) {
                ll newShortestDist = djikstra(n, edges, src, dstn);

                if (newShortestDist <= target) {
                    matchedTarget = true;
                    edge[2] += (target - newShortestDist);
                }
            }
        }

        if (matchedTarget)
            return edges;

        return {};
    }
};
```