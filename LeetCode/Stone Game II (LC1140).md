---
difficulty: medium
leetcode-num: 1140
topics:
  - Array
  - Dynamic Programming
  - Game Theory
---
[Problem Link](https://leetcode.com/problems/stone-game-ii/)

#### Problem
Alice and Bob continue their games with piles of stones.  There are a number of piles **arranged in a row**, and each pile has a positive integer number of stones `piles[i]`.  The objective of the game is to end with the most stones. 

Alice and Bob take turns, with Alice starting first.  Initially, `M = 1`.

On each player's turn, that player can take **all the stones** in the **first** `X` remaining piles, where `1 <= X <= 2M`.  Then, we set `M = max(M, X)`.

The game continues until all the stones have been taken.

Assuming Alice and Bob play optimally, return the maximum number of stones Alice can get.

**Example 1:**

**Input:** piles = `[2,7,9,4,4]`
**Output:** 10
**Explanation:**  If Alice takes one pile at the beginning, Bob takes two piles, then Alice takes 2 piles again. Alice can get 2 + 4 + 4 = 10 piles in total. If Alice takes two piles at the beginning, then Bob can take all three piles left. In this case, Alice get 2 + 7 = 9 piles in total. So we return 10 since it's larger. 

**Example 2:**

**Input:** piles = `[1,2,3,4,5,100]`
**Output:** 104

**Constraints:**

- `1 <= piles.length <= 100`
- `1 <= piles[i] <= 104`

#### Solution


##### Memoization Approach
[Video Explanation](https://youtu.be/9f1vzDFVnGA)

*TC ->* O( n$^{3}$  )
*SC ->* O( n$^{2}$ )

```cpp title=Code
class Solution {
    int n;
    int dp[2][101][101];
public:
    int solveForAlice(vector<int>& piles, int person, int i , int M ){
        if( i >= n )
            return 0;

        if(dp[person][i][M] != -1)
            return dp[person][i][M];
            
        int result = (person == 1 )?-1 : INT_MAX;

        int stones = 0;

        for( int x = 1 ; x <= min(2*M, n - i); x++){
            stones+= piles[i+x-1];

            if(person == 1){ // Alice
                result = max( result, stones +  solveForAlice(piles,0,i+x , max(x,M)));

            }else{
                result = min( result, solveForAlice(piles,1,i+x, max(x,M)));
            }
        }
        return dp[person][i][M] = result;
    }

    int stoneGameII(vector<int>& piles) {
        n = piles.size();
        memset(dp,-1,sizeof(dp));

        return solveForAlice(piles,1,0,1);
    }
};
```

##### Tabulation Approach

This solution uses a bottom-up approach with tabulation. Here's how it works:

1. We create a 2D DP table where `dp[i][m]` represents the maximum number of stones Alice can get starting from index `i` with a current M value of `m`.
2. We also calculate a suffix sum array to quickly get the sum of stones from any index to the end.
3. We fill the DP table from right to left (from the end of the piles to the start), and for each position, we consider all possible M values.
4. For each state (i, m):
    - If we can take all remaining stones` (i + 2*m >= n)`, we do so.
    - Otherwise, we try all possible X values `(1 to 2*m)` and choose the one that maximizes our score.
5. The final answer is stored in `dp[0][1]`, which represents the maximum score Alice can get starting from the beginning with an initial M value of 1.

*TC ->* O( n$^{3}$  )
*SC ->* O( n$^{2}$ )

```cpp title=Code
class Solution {
public:
    int stoneGameII(vector<int>& piles) {
        int n = piles.size();
        vector<int> suffixSum(n + 1, 0);
        vector<vector<int>> dp(n + 1, vector<int>(n + 1, 0));
        
        // Calculate suffix sum
        for (int i = n - 1; i >= 0; i--) {
            suffixSum[i] = suffixSum[i + 1] + piles[i];
        }
        
        // Fill dp table
        for (int i = n - 1; i >= 0; i--) {
            for (int m = 1; m <= n; m++) {
                if (i + 2 * m >= n) {
                    dp[i][m] = suffixSum[i];
                } else {
                    for (int x = 1; x <= 2 * m; x++) {
                        int opponentScore = dp[i + x][max(m, x)];
                        int score = suffixSum[i] - opponentScore;
                        dp[i][m] = max(dp[i][m], score);
                    }
                }
            }
        }
        
        return dp[0][1];
    }
};
```