---
difficulty: easy
topics:
  - Math
---
[Problem Link](https://takeuforward.org/data-structure/program-to-generate-pascals-triangle/) ( This is variation 1 in the problem article)

#### Problem

Given row number r and column number c. Print the element at position (r, c) in Pascal’s triangle.

#### Solution
[Video Explanation]https://youtu.be/bR7mQgwQ_o8?t=256)

##### Optimal Approach

In a Pascal Triangle, the c$^{th}$ element of the r$^{th}$ row in a Pascal triangle is:  $^{r-1}$C$_{c-1}$

You must remember this fact for a Pascal Triangle as this formula gives us the optimal solution to this problem. 

![[Pascal's Triangle III (Finding nCr in minimal time) 2024-07-17 16.04.06.excalidraw|650]]

*TC ->* O( c ) , where c is the column number.
*SC ->* O( 1 )

```cpp title=Code
int nCr(int n, int r) {
    long long ans = 1;
    for (int i = 0; i < r; i++) {
        ans = ans * (n - i);
        ans = ans / (i + 1);
    }
    return ans;
}

int pascalElement(int row, int column) {
    return nCr(row - 1, column - 1);
}
```

##### Brute Force Approach
Generate the entire Pascal Triangle like we did in [[Pascal's Triangle (LC118)]] and return the required element.

*TC ->* O(  n$^{2}$ )
*SC ->* O( n$^{2}$ )

#### Similar Problems
- [[Pascal's Triangle (LC118)]]
- [[Pascal's Triangle II (LC119)]]