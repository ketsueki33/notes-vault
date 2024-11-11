---
difficulty: easy
leetcode-num: 2490
topics:
  - String
---
[Problem Link](https://leetcode.com/problems/circular-sentence/)

#### Problem
A **sentence** is a list of words that are separated by a **single** space with no leading or trailing spaces.

- For example, `"Hello World"`, `"HELLO"`, `"hello world hello world"` are all sentences.

Words consist of **only** uppercase and lowercase English letters. Uppercase and lowercase English letters are considered different.

A sentence is **circular** if:

- The last character of a word is equal to the first character of the next word.
- The last character of the last word is equal to the first character of the first word.

For example, `"leetcode exercises sound delightful"`, `"eetcode"`, `"leetcode eats soul"` are all circular sentences. However, `"Leetcode is cool"`, `"happy Leetcode"`, `"Leetcode"` and `"I like Leetcode"` are **not** circular sentences.

Given a string `sentence`, return `true` _if it is circular_. Otherwise, return `false`.

**Example 1:**

**Input:** sentence = "leetcode exercises sound delightful"
**Output:** true
**Explanation:** The words in sentence are ["leetcode", "exercises", "sound", "delightful"].
- leetcode's last character is equal to exercises's first character.
- exercises's last character is equal to sound's first character.
- sound's last character is equal to delightful's first character.
- delightful's last character is equal to leetcode's first character.
The sentence is circular.

**Example 2:**

**Input:** sentence = "eetcode"
**Output:** true
**Explanation:** The words in sentence are ["eetcode"].
- eetcode's last character is equal to eetcode's first character.
The sentence is circular.

**Example 3:**

**Input:** sentence = "Leetcode is cool"
**Output:** false
**Explanation:** The words in sentence are ["Leetcode", "is", "cool"].
- Leetcode's last character is **not** equal to is's first character.
The sentence is **not** circular.

**Constraints:**

- `1 <= sentence.length <= 500`
- `sentence` consist of only lowercase and uppercase English letters and spaces.
- The words in `sentence` are separated by a single space.
- There are no leading or trailing spaces.

#### Solution
##### Optimal Approach (without stringstream)

*TC ->* O( n )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    bool isCircularSentence(string sntc) {

        if (sntc.front() != sntc.back())
            return false;

        for (int i = 0; i < sntc.size(); i++) {
            if (sntc[i] == ' ' && sntc[i - 1] != sntc[i + 1])
                return false;
        }

        return true;
    }
};
```

##### Alright Approach (stringstream)

- **Setup and Initial Checks**:
    
    - We first define an empty `string word` to store each word in the sentence as we iterate through it.
    - `lastCh` is initialized with the last character of the sentence (`sntc.back()`)—this is because, in a circular sentence, the first word's first letter should match the last letter of the last word.
- **Extracting Words with `stringstream`**:
    
    - We use `stringstream ss(sntc)` to split the sentence into individual words by whitespace.
    - Each word is stored in `word` as we iterate using `ss >> word`.
- **Checking Circularity**:
    
    - For each word, we check if `word.front()`, the first character of the current word, matches `lastCh`, which initially holds the last character of the sentence.
        - If there’s a mismatch, we return `false` immediately because the circular condition is broken.
    - After each check, `lastCh` is updated to the last character of the current word (`word.back()`), setting up the comparison for the next word.
- **Return Result**:
    
    - If all words match the circular criteria, the loop completes, and we return `true`, indicating the sentence is circular.


*TC ->* O( n )
*SC ->* O( n ) , ( to store the stringstream)

```cpp title=Code
```
