---
difficulty: easy
leetcode-num: 1684
topics:
  - Hash Table
  - String
  - Bit Manipulation
---
[Problem Link](https://leetcode.com/problems/count-the-number-of-consistent-strings/)

#### Problem
You are given a string `allowed` consisting of **distinct** characters and an array of strings `words`. A string is **consistent** if all characters in the string appear in the string `allowed`.

Return _the number of **consistent** strings in the array_ `words`.

**Example 1:**

**Input:** allowed = "ab", words = `["ad","bd","aaab","baa","badab"]`
**Output:** 2
**Explanation:** Strings "aaab" and "baa" are consistent since they only contain characters 'a' and 'b'.

**Example 2:**

**Input:** allowed = "abc", words = `["a","b","c","ab","ac","bc","abc"]`
**Output:** 7
**Explanation:** All strings are consistent.

**Example 3:**

**Input:** allowed = "cad", words = `["cc","acd","b","ba","bac","bad","ac","d"]`
**Output:** 4
**Explanation:** Strings "cc", "acd", "ac", and "d" are consistent.

**Constraints:**

- `1 <= words.length <= 104`
- `1 <= allowed.length <= 26`
- `1 <= words[i].length <= 10`
- The characters in `allowed` are **distinct**.
- `words[i]` and `allowed` contain only lowercase English letters.

#### Solution

##### Bit Manipulation - Optimal Approach
[Video Explanation](https://youtu.be/QEIKg95HKzg)

- **Creating a Mask for Allowed Characters:**    
    - A **bitmask** is used to represent the characters allowed in the words.
    - The `allowed` string contains the characters that can appear in the words.
    - For each character in `allowed`, the corresponding bit in the bitmask `mask` is set to `1`. This allows us to efficiently check if a character is allowed by using bitwise operations.
- **Processing Each Word:**    
    - For each word in the `words` list, the `processWord` function is called to determine if the word contains only allowed characters.
    - The function checks each character of the word by shifting the corresponding bit (based on the character) in the bitmask. If the bit is not set (i.e., the character is not allowed), the function returns `false`.
- **Counting Valid Words:**    
    - The program counts the number of valid words, i.e., words where every character exists in the `allowed` string.

*TC ->* O(  `m + n*k` )
*SC ->* O( 1 )

where m is the length of allowed string, n is number of words, and k is length of each word

```cpp title=Code
class Solution {
    bool processWord(string word, int mask){
        for( char& ch : word ){
            if( (( mask >> (ch - 'a') ) & 1) == 0 )
                return false;
        }
        return true;
    }

public:
    int countConsistentStrings(string allowed, vector<string>& words) {
        int mask = 0 ;
        int count = 0;

        for( char x : allowed)
            mask = (mask | 1<<(x-'a'));

        for( string &word : words )
            if(processWord(word,mask))
                count++;

        return count;
    }
};
```

##### HashMap Approach

```cpp title=Code
class Solution {
    bool processWord(string word, unordered_set<char> s) {
        for (char x : word) {
            if (s.count(x) == 0)
                return false;
        }
        return true;
    }

public:
    int countConsistentStrings(string allowed, vector<string>& words) {
        unordered_set<char> s;

        for (char x : allowed)
            s.insert(x);

        int count = 0;

        for (string x : words) {
            if (processWord(x, s))
                count++;
        }
        return count;
    }
};
```