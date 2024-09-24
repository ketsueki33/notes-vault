---
difficulty: easy
leetcode-num: 884
topics:
  - String
  - Hash Table
---
[Problem Link](https://leetcode.com/problems/uncommon-words-from-two-sentences/)

#### Problem
A **sentence** is a string of single-space separated words where each word consists only of lowercase letters.

A word is **uncommon** if it appears exactly once in one of the sentences, and **does not appear** in the other sentence.

Given two **sentences** `s1` and `s2`, return _a list of all the **uncommon words**_. You may return the answer in **any order**.

**Example 1:**

**Input:** s1 = "this apple is sweet", s2 = "this apple is sour"

**Output:** `["sweet","sour"]`

**Explanation:**

The word `"sweet"` appears only in `s1`, while the word `"sour"` appears only in `s2`.

**Example 2:**

**Input:** s1 = "apple apple", s2 = "banana"

**Output:** `["banana"]`

**Constraints:**

- `1 <= s1.length, s2.length <= 200`
- `s1` and `s2` consist of lowercase English letters and spaces.
- `s1` and `s2` do not have leading or trailing spaces.
- All the words in `s1` and `s2` are separated by a single space.

#### Solution
- **Using a HashMap for Counting**:    
    - A hash map (`unordered_map<string, int>`) is used to store the frequency of each word from both sentences (`s1` and `s2`).
    - First, both sentences are combined into a single string using stringstream, and then words are extracted one by one to be added to the map.
- **Processing Words**:    
    - The words from both sentences are split and inserted into the map. Each word is the key, and the value is the count of occurrences.
- **Identifying Uncommon Words**:    
    - After the map is built, we loop through it and check for words that have a count of exactly `1`, meaning they appear only once across both sentences. These words are pushed into the result vector `res`.
- **Return the Result**:    
    - The vector `res` containing all the uncommon words is returned as the output.

*TC ->* O( n ), where n is total number of words in s1 + s2
*SC ->* O( n )

```cpp title=Code
class Solution {
public:
    vector<string> uncommonFromSentences(string s1, string s2) {
        unordered_map<string, int> mp;
        vector<string> res;

        stringstream ss(s1 + " " + s2);
        string curr;

        while (ss >> curr) {
            mp[curr]++;
        }

        for (auto& [word, count] : mp) {
            if (count == 1)
                res.push_back(word);
        }

        return res;
    }
};
```