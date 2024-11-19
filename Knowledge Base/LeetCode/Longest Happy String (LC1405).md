---
difficulty: medium
leetcode-num: 1405
topics:
  - String
  - Greedy
  - Heap
---
[Problem Link](https://leetcode.com/problems/longest-happy-string/)

#### Problem
A string `s` is called **happy** if it satisfies the following conditions:

- `s` only contains the letters `'a'`, `'b'`, and `'c'`.
- `s` does not contain any of `"aaa"`, `"bbb"`, or `"ccc"` as a substring.
- `s` contains **at most** `a` occurrences of the letter `'a'`.
- `s` contains **at most** `b` occurrences of the letter `'b'`.
- `s` contains **at most** `c` occurrences of the letter `'c'`.

Given three integers `a`, `b`, and `c`, return _the **longest possible happy** string_. If there are multiple longest happy strings, return _any of them_. If there is no such string, return _the empty string_ `""`.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** a = 1, b = 1, c = 7
**Output:** "ccaccbcc"
**Explanation:** "ccbccacc" would also be a correct answer.

**Example 2:**

**Input:** a = 7, b = 1, c = 0
**Output:** "aabaa"
**Explanation:** It is the only correct answer in this case.

**Constraints:**

- `0 <= a, b, c <= 100`
- `a + b + c > 0`

#### Solution
[Video Explanation](https://youtu.be/tGzgghQEDdA)

##### Optimal Approach
###### Approach Breakdown:

1. **Priority Queue**:
    
    - The solution uses a **max-heap** (priority queue) to always prioritize the character with the highest count. This ensures that the most available character is added to the result as long as it maintains the no-three-consecutive rule.
    - The heap stores pairs where the first element is the count of the character and the second element is the character itself, i.e., `{count, character}`.
2. **`isValid` function**:
    
    - This function checks whether appending the current character to the result string would violate the "no three consecutive characters" rule.
    - If the last two characters in the string are the same as the current character, the function returns `false` (invalid); otherwise, it returns `true`.
3. **Building the String**:
    
    - Initially, the counts of `'a'`, `'b'`, and `'c'` are pushed into the priority queue if they are greater than zero.
    - The process then proceeds in a loop, picking the character with the highest count from the queue (since it’s a max-heap).
        - If adding this character doesn’t violate the rule (`isValid` returns `true`), it's appended to the result string, and its count is decremented.
        - If adding it violates the rule (`isValid` returns `false`), the next most frequent character is chosen instead. This helps break the chain of consecutive characters.
        - After using the second character, the first character is pushed back into the priority queue to be used later.
4. **Edge Cases**:
    
    - If the current character can’t be added and there’s no other character available in the priority queue, the loop breaks since it’s impossible to continue without violating the rules.

###### Step-by-Step Execution:

1. **Initialization**:
    
    - We create a max-heap of character counts for `'a'`, `'b'`, and `'c'`, pushing them into the queue if their counts are greater than zero.
2. **While Loop**:
    
    - The loop continues as long as there are characters left in the priority queue.
    - The most frequent character is selected and checked using `isValid`. If it can be added without breaking the consecutive rule, it’s appended to the result string.
    - If not, the next most frequent character is added, and the current character is pushed back into the queue to be used later when it's valid.
3. **End of Loop**:
    
    - The loop ends either when no more characters can be added (i.e., all characters are exhausted or adding any remaining characters would violate the rules).

*TC ->* O( `(a + b + c) * log 3` ) = O(a + b + c)
*SC ->* O( 1 ) for storing the result string and the heap of fixed size 3

```cpp title=Code
class Solution {
    bool isValid(string& s, char ch) {
        if (s.size() < 2)
            return true;

        if (s[s.size() - 1] == ch && s[s.size() - 2] == ch)
            return false;

        return true;
    }

public:
    string longestDiverseString(int a, int b, int c) {
        priority_queue<pair<int, char>> pq;
        string res = "";

        if (a > 0)
            pq.push({a, 'a'});
        if (b > 0)
            pq.push({b, 'b'});
        if (c > 0)
            pq.push({c, 'c'});

        while (!pq.empty()) {
            auto curr = pq.top();
            pq.pop();

            if (isValid(res, curr.second)) {
                res += curr.second;

                if (--curr.first > 0)
                    pq.push(curr);

            } else if (!pq.empty()) {
                auto next = pq.top();
                pq.pop();

                res += next.second;

                if (--next.first > 0)
                    pq.push(next);

                pq.push(curr);
            } else
                break;
        }

        return res;
    }
};
```