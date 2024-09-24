---
difficulty: medium
leetcode-num: 2028
topics:
  - Math
  - Array
  - Simulation
---
[Problem Link](https://leetcode.com/problems/find-missing-observations/)

#### Problem
You have observations of `n + m` **6-sided** dice rolls with each face numbered from `1` to `6`. `n` of the observations went missing, and you only have the observations of `m` rolls. Fortunately, you have also calculated the **average value** of the `n + m` rolls.

You are given an integer array `rolls` of length `m` where `rolls[i]` is the value of the `ith` observation. You are also given the two integers `mean` and `n`.

Return _an array of length_ `n` _containing the missing observations such that the **average value** of the_ `n + m` _rolls is **exactly**_ `mean`. If there are multiple valid answers, return _any of them_. If no such array exists, return _an empty array_.

The **average value** of a set of `k` numbers is the sum of the numbers divided by `k`.

Note that `mean` is an integer, so the sum of the `n + m` rolls should be divisible by `n + m`.

**Example 1:**

**Input:** rolls = `[3,2,4,3]`, mean = 4, n = 2
**Output:** `[6,6]`
**Explanation:** The mean of all n + m rolls is (3 + 2 + 4 + 3 + 6 + 6) / 6 = 4.

**Example 2:**

**Input:** rolls = `[1,5,6`], mean = 3, n = 4
**Output:** `[2,3,2,2]`
**Explanation:** The mean of all n + m rolls is (1 + 5 + 6 + 2 + 3 + 2 + 2) / 7 = 3.

**Example 3:**

**Input:** rolls = `[1,2,3,4]`, mean = 6, n = 4
**Output:** `[]`
**Explanation:** It is impossible for the mean to be 6 no matter what the 4 missing rolls are.

**Constraints:**

- `m == rolls.length`
- `1 <= n, m <= 105`
- `1 <= rolls[i], mean <= 6`

#### Solution
[Video Explanation](https://youtu.be/86yKkaNi3sU)

**1. Calculate the Total Sum (`sumOfAll`)**:
 The total sum of all dice rolls (both existing and missing) can be calculated using the formula: 
```
sumOfAll=mean×(m+n)
```
 
where `m` is the number of existing dice rolls (length of `rolls`), and `n` is the number of missing rolls.

**2. Calculate the Sum of Existing Rolls (`sumOfM`)**:   

 You loop through the `rolls` array and sum up all the values. This gives you the sum of existing dice rolls, `sumOfM`.
 
**3. Find the Sum of Missing Rolls (`sumOfN`)**:    
Once you know `sumOfM`, you can find the sum of the missing rolls by subtracting `sumOfM` from `sumOfAll`: 
```
sumOfN=sumOfAll−sumOfM
```
Now you need to distribute this `sumOfN` across the `n` missing dice rolls, ensuring that each value is between 1 and 6 (since it's a dice roll).

**4. Distribute the Sum of Missing Rolls (`makeSum`)**:

First, check if it's possible to distribute `sumLeft` across the `spotsLeft`. If not (because `sumLeft` is too big or too small), return an empty result.

If the number of remaining spots equals the remaining sum (`spotsLeft == sumLeft`), it means each remaining spot must have a value of `1`. Push all `1`s into the result and break out of the loop.

Otherwise, find the maximum possible value for the current spot (`min(sumLeft - spotsLeft + 1, 6)`) and push it into the result.

Decrease `sumLeft` by this value, and reduce `spotsLeft` by 1, repeating the process until all spots are filled.

*TC ->* O( n )
*SC ->* O( 1 ) if we exclude the result vector

```cpp title=Code
class Solution {

    vector<int> makeSum(int sumLeft, int spotsLeft) {
        vector<int> res;

        if (sumLeft > spotsLeft * 6 || spotsLeft > sumLeft)
            return res; // return empty res if solution not possible

        while (spotsLeft > 0) {
            // fill all remaining spots with 1 if it's equal to sum
            if (spotsLeft == sumLeft) {
                res.insert(res.end(), spotsLeft, 1);
                break;
            }

            int nextVal = min(sumLeft - spotsLeft + 1, 6);
            res.push_back(nextVal);
            sumLeft -= nextVal;
            spotsLeft--;
        }

        return res;
    }

public:
    vector<int> missingRolls(vector<int>& rolls, int mean, int n) {
        int m = rolls.size();
        int sumOfAll = mean * (n + m);

        int sumOfM = 0, sumOfN;

        for (int x : rolls) {
            sumOfM += x;
        }

        sumOfN = sumOfAll - sumOfM;

        return makeSum(sumOfN, n);
    }
};
```