---
difficulty: medium
leetcode-num: 122
topics:
  - Array
  - Dynamic Programming
  - Greedy
---
[Problem Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

#### Problem
You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `i`$^{th}$ day. 

On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can buy it then immediately sell it on the **same day**.

Find and return _the **maximum** profit you can achieve_.

**Example 1:**

**Input:** prices = [7,1,5,3,6,4]
**Output:** 7
**Explanation:** Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.

**Example 2:**

**Input:** prices = [1,2,3,4,5]
**Output:** 4
**Explanation:** Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Total profit is 4.

**Example 3:**

**Input:** prices = [7,6,4,3,1]
**Output:** 0
**Explanation:** There is no way to make a positive profit, so we never buy the stock to achieve the maximum profit of 0.

#### Solution
##### Greedy Approach ( easier & optimal )
We will initialize three variables:-
- `curr_day` to store price of current day inside the loop
- `prev_day` to store price of previous day inside the loop
- `profit` to count the profit made so far

On every day we will compare `prev_day` and `curr_day`. Only and only if `prev_day` is less than `curr_day` will we **add `prev_day` - `curr_day`  to `profit`**. 

Think as if there is an arrow pointing from `prev_day` to `curr_day` and we are only buying on previous day and selling on current day if the arrow is pointing upwards.

![[Stock Buy and Sell II (LC122) 2024-06-29 01.59.37.excalidraw|500]]

This will also work if the stock prices keep increasing for consecutive days because this algorithm always assumes you have bought the previous day stock ( but the stock isn't bought till our algorithm decides to sell on current day ). Which means, even if you sold a stock for  `x` amt of profit.. and the price increases by `y` the next day.. the algorithm will consider that you bought the stock immediately after selling on the previous day. So, the total profit will be `x` + `y`.

![[Stock Buy and Sell II (LC122) 2024-06-29 02.17.53.excalidraw]]

TC -> `O( n )`
SC -> `O( 1 )`

```cpp title=Code
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int curr_day = prices[0];
        int prev_day = INT_MAX;

        int profit = 0;

        for (int i = 1; i < prices.size(); i++) {
            prev_day = curr_day;
            curr_day = prices[i];

            if (prev_day < curr_day)
                profit += curr_day - prev_day;
        }
        return profit;
    }
};
```

##### DP Approach
[Video Explanation](https://youtu.be/nGJmxkUJQGs)

Solution taken from this [TakeUForward Article](https://takeuforward.org/data-structure/buy-and-sell-stock-ii-dp-36/)

This is the recursion approach in DP.

Every day, we will have two choices, either to do nothing and move to the next day or to buy/sell (based on the last transaction) and find out the profit. Therefore we need to generate all the choices in order to compare the profit.

**Step 1:** Express the problem in terms of indexes.

We need to think in the terms of the number of days, therefore one variable will be the array index( say ind). Next, we need to respect the condition that we can’t buy a stock again, that is we need to first sell a stock, and then we can buy that again. Therefore we need a second variable ‘buy’ which tells us on a particular day whether we can buy or sell the stock. We can generalize the function as :

**Step 2:** Try out all possible choices at a given index.

Every day, we have two choices:

- To either buy/sell the stock(based on the buy variable’s value). 
- To do nothing and move on to the next day.

We need to generate all the choices. We will use the pick/non-pick technique.

*Case 1:* When buy == 0, we can buy the stock.

If we can buy the stock on a particular day, we have two options:

- **Option 1:** To do no transaction and move to the next day. In this case, the net profit earned will be **0** from the current transaction, and to calculate the maximum profit starting from the next day, we will recursively call f(ind+1,0). As we have not bought the stock, the ‘buy’ variable value will still remain 0, indicating that we can buy the stock the next day.

- **Option 2:** The other option is to buy the stock on the current day. In this case, the net profit earned from the current transaction will be **-Arr[i]**. As we are buying the stock, we are giving money out of our pocket, therefore the profit we earn **is negative**. To calculate the maximum profit starting from the next day, we will recursively call f(ind+1,1). As we have bought the stock, the ‘buy’ variable value will change to 1, indicating that we can’t buy and only sell the stock the next day.

*Case 2:* When buy == 1, we can sell the stock.

If we can buy the stock on a particular day, we have two options:

- **Option 1:** To do no transaction and move to the next day. In this case, the net profit earned will be **0** from the current transaction, and to calculate the maximum profit starting from the next day, we will recursively call **f(ind+1,1)**. As we have not bought the stock, the ‘buy’ variable value will still remain at 1, indicating that we can’t buy and only sell the stock the next day.

- **Option 2:** The other option is to sell the stock on the current day. In this case, the net profit earned from the current transaction will be +**Arr[i]**. As we are selling the stock, we are putting the money into our pocket, therefore the profit we earn **is positive**. To calculate the maximum profit starting from the next day, we will recursively call **f(ind+1,0)**. As we have sold the stock, the ‘buy’ variable value will change to 0, indicating that we can buy the stock the next day.

**Step 3:  Return the maximum** 

As we are looking to maximize the profit earned, we will return the maximum value in both cases.

**Steps to memoize**:

If we draw the recursion tree, we will see that there are overlapping subproblems. In order to convert a recursive solution the following steps will be taken:

1. Create a dp array of size `[n][2]`. The size of the input array is ‘n’, so the index will always lie between ‘0’ and ‘n-1’. The ‘buy’  variable can take only two values: 0 and 1. Therefore we take the dp array as `dp[n][2]`
2. We initialize the dp array to -1.
3. Whenever we want to find the answer to particular parameters (say f(ind, buy)), we first check whether the answer is already calculated using the dp array(i.e `dp[ind][buy]`!= -1 ). If yes, simply return the value from the dp array.
4. If not, then we are finding the answer for the given value for the first time, we will use the recursive relation as usual but before returning from the function, we will set `dp[ind][buy]` to the solution we get.

TC -> `O( n )`
SC -> `O( n )`

```cpp title=Code
long getAns(long *Arr, int ind, int buy, int n, vector<vector<long>> &dp) {
    // Base case: When we reach the end of the array, there are no more decisions to make.
    if (ind == n) {
        return 0;
    }

    // If the result for this state has already been calculated, return it
    if (dp[ind][buy] != -1) {
        return dp[ind][buy];
    }

    long profit = 0;

    if (buy == 0) { // We can buy the stock
        profit = max(0 + getAns(Arr, ind + 1, 0, n, dp), -Arr[ind] + getAns(Arr, ind + 1, 1, n, dp));
    }

    if (buy == 1) { // We can sell the stock
        profit = max(0 + getAns(Arr, ind + 1, 1, n, dp), Arr[ind] + getAns(Arr, ind + 1, 0, n, dp));
    }

    // Store the calculated profit in the DP table and return it
    return dp[ind][buy] = profit;
}

long getMaximumProfit(long *Arr, int n) {
    // Create a DP table to memoize results
    vector<vector<long>> dp(n, vector<long>(2, -1));

    if (n == 0) {
        return 0;
    }

    long ans = getAns(Arr, 0, 0, n, dp);
    return ans;
}
```
#### Similar Problems

- [[Stock Buy and Sell I (LC121)]]