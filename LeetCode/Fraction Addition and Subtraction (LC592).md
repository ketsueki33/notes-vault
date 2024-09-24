---
difficulty: medium
leetcode-num: 592
topics:
  - String
  - Math
---
[Problem Link](https://leetcode.com/problems/fraction-addition-and-subtraction/)

#string-stream
#### Problem
Given a string `expression` representing an expression of fraction addition and subtraction, return the calculation result in string format.

The final result should be an [irreducible fraction](https://en.wikipedia.org/wiki/Irreducible_fraction). If your final result is an integer, change it to the format of a fraction that has a denominator `1`. So in this case, `2` should be converted to `2/1`.

**Example 1:**

**Input:** expression = "-1/2+1/2"
**Output:** "0/1"

**Example 2:**

**Input:** expression = "-1/2+1/2+1/3"
**Output:** "1/3"

**Example 3:**

**Input:** expression = "1/3-1/2"
**Output:** "-1/6"

**Constraints:**

- The input string only contains `'0'` to `'9'`, `'/'`, `'+'` and `'-'`. So does the output.
- Each fraction (input and output) has the format `±numerator/denominator`. If the first input fraction or the output is positive, then `'+'` will be omitted.
- The input only contains valid **irreducible fractions**, where the **numerator** and **denominator** of each fraction will always be in the range `[1, 10]`. If the denominator is `1`, it means this fraction is actually an integer in a fraction format defined above.
- The number of given fractions will be in the range `[1, 10]`.
- The numerator and denominator of the **final result** are guaranteed to be valid and in the range of **32-bit** int.

#### Solution

##### Optimal Approach

*TC ->* O(  )
*SC ->* O(  )

```cpp title=Code
class Solution {
    static int gcd(int a, int b) {
        a = abs(a);
        b = abs(b);

        while (b != 0) {
            int temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }
    struct Fraction {
        int numer;
        int denom;

        Fraction(int a, int b) : numer(a), denom(b) {}

        string stringify() { return to_string(numer) + "/" + to_string(denom); }

        void simplify() {
            int div = gcd(numer, denom);
            numer /= div;
            denom /= div;
        }
    };

    queue<Fraction> convert(string& expression) {
        queue<Fraction> fraction;
        stringstream ss(expression);
        char op;
        int num, denom;

        while (ss >> num >> op >> denom) {
            fraction.push(Fraction(num, denom));
            //    cout<<num<<","<<op<<denom<<endl;
        }
        return fraction;
    }

public:
    string fractionAddition(string expression) {
        queue<Fraction> q;
        int sign = 1;
        q = convert(expression);

        while (q.size() != 1) {
            Fraction f1 = q.front();
            q.pop();
            Fraction f2 = q.front();
            q.pop();

            cout << f1.stringify() << "+" << f2.stringify() << "=";

            f1.numer *= f2.denom;
            f2.numer *= f1.denom;

            Fraction f3(f1.numer + f2.numer, f2.denom * f1.denom);
            cout << f3.stringify() << endl;
            q.push(f3);
        }
        Fraction f = q.front();
        f.simplify();
        return f.stringify();
    }
};
```