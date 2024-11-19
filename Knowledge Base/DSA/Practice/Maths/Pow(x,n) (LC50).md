---
difficulty: medium
leetcode-num: 50
topics:
  - Math
  - Recursion
---
[Problem Link](https://leetcode.com/problems/powx-n/)

#### Problem
Implement [[x, n)](http://www.cplusplus.com/reference/valarray/pow/|pow(x, n)]], which calculates `x` raised to the power `n` (i.e., `x^n`).

**Example 1:**

**Input:** x = 2.00000, n = 10
**Output:** 1024.00000

**Example 2:**

**Input:** x = 2.10000, n = 3
**Output:** 9.26100

**Example 3:**

**Input:** x = 2.00000, n = -2
**Output:** 0.25000
**Explanation:** 2-2 = 1/22 = 1/4 = 0.25

**Constraints:**

- `-100.0 < x < 100.0`
- `-2^31 <= n <= 2^31-1`
- `n` is an integer.
- Either `x` is not zero or `n > 0`.
- `-10^4 <= xn <= 10^4`

#### Solution
##### Iterative Approach (Optimal)
[Video Explanation](https://youtu.be/l0YC3876qxg)

We will initialize two variables:
- `nn` to store the power `n`. We are storing `n` in a `long`  type to avoid overflow later when we multiply it with `-1` to make it positive.
- `ans` to store the ans.

 We will solve this inside the while loop by taking two cases:
1. `nn` is even: square `x `and divide `nn` by 2
2. `nn` is odd: multiply `ans `with `x` and reduce `nn` by 1.

We exit the while loop when `nn` reaches 0.

*TC ->* O( log$_{2}$ n )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    double myPow(double x, int n) {
        long nn = n;
        double ans = 1.0;
        if (n < 0) {
            nn *= -1;
            x = 1 / x;
        }
        // we will solve this inside the while loop by taking two cases:
        //  1. nn is even: square x and divide nn by 2
        //  2. nn is odd: multiply ans with x and reduce nn by 1.
        // this will stop when nn reaches 0
        while (nn) {
            if (nn % 2 == 0) {
                x *= x;
                nn /= 2;
            } else {
                ans *= x;
                nn--;
            }
        }

        return ans;
    }
};
```

##### Recursive Approach

*TC ->* O( log$_{2}$ n )
*SC ->* O( log$_{2}$ n )

```cpp title=Code
class Solution {
public:
    double myPow(double x, int n) {
        if(n<0){
            x = 1/x;
            n = abs(n);
        }
        if(n==0){
            return 1;
        }
        if(n%2 == 0){
            double p = myPow(x, n/2);
            return p*p;
        }
        else{
            return x * myPow(x,n-1);
        }
    }
};
```