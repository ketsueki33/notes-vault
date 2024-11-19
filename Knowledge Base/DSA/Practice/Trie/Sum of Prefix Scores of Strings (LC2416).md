---
difficulty: hard
leetcode-num: 2416
topics:
  - String
  - Trie
---
[Problem Link](https://leetcode.com/problems/sum-of-prefix-scores-of-strings/)

#### Problem
You are given an array `words` of size `n` consisting of **non-empty** strings.

We define the **score** of a string `word` as the **number** of strings `words[i]` such that `word` is a **prefix** of `words[i]`.

- For example, if `words = ["a", "ab", "abc", "cab"]`, then the score of `"ab"` is `2`, since `"ab"` is a prefix of both `"ab"` and `"abc"`.

Return _an array_ `answer` _of size_ `n` _where_ `answer[i]` _is the **sum** of scores of every **non-empty** prefix of_ `words[i]`.

**Note** that a string is considered as a prefix of itself.

**Example 1:**

**Input:** `words = ["abc","ab","bc","b"]`
**Output:** `[5,4,3,2]`
**Explanation:** The answer for each string is the following:
- "abc" has 3 prefixes: "a", "ab", and "abc".
- There are 2 strings with the prefix "a", 2 strings with the prefix "ab", and 1 string with the prefix "abc".
The total is `answer[0] = 2 + 2 + 1 = 5`.
- "ab" has 2 prefixes: "a" and "ab".
- There are 2 strings with the prefix "a", and 2 strings with the prefix "ab".
The total is `answer[1] = 2 + 2 = 4`.
- "bc" has 2 prefixes: "b" and "bc".
- There are 2 strings with the prefix "b", and 1 string with the prefix "bc".
The total is `answer[2] = 2 + 1 = 3`.
- "b" has 1 prefix: "b".
- There are 2 strings with the prefix "b".
The total is `answer[3] = 2`.

**Example 2:**

**Input:** `words = ["abcd"]`
**Output:** `[4]`
**Explanation:**
"abcd" has 4 prefixes: "a", "ab", "abc", and "abcd".
Each prefix has a score of one, so the total is `answer[0] = 1 + 1 + 1 + 1 = 4.`

**Constraints:**

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 1000`
- `words[i]` consists of lowercase English letters.

#### Solution
[Video Explanation](https://youtu.be/PRHedN3h2Gc)

##### Optimal Approach

1. **TrieNode Structure:**
    
    - The `TrieNode` struct represents each node in a Trie (prefix tree).
    - It has:
        - `count`: This keeps track of how many words pass through this node, representing how many words have this prefix.
        - `children[26]`: This array of size 26 corresponds to the 26 lowercase English letters ('a' to 'z'). Each index of this array points to another TrieNode.
2. **Insert Function:**
    
    - The `insert` function takes a word and inserts it into the Trie.
    - For each character in the word:
        - It calculates the index `idx` corresponding to the character by subtracting 'a' from the character (e.g., 'a' becomes 0, 'b' becomes 1).
        - If the corresponding child node at `idx` is `nullptr`, a new `TrieNode` is created.
        - We move to this child node and increment its `count`, indicating that a word passed through this node (i.e., this prefix was encountered).
3. **getScore Function:**
    
    - This function is used to calculate the prefix score for a given word.
    - It traverses the Trie using the characters of the word, following the child nodes.
    - As it traverses, it adds the `count` of each node it visits to a variable `preCount`. This gives the sum of how many times each prefix of the word has appeared in all words.
4. **sumPrefixScores Function:**
    
    - This is the main function that calculates the prefix scores for all the words.
    - It first creates the Trie by inserting each word from the `words` vector using the `insert` function.
    - Then, for each word, it calculates its prefix score using the `getScore` function and stores the result in the `res` vector.

###### Key Steps:
- **Trie Construction:** The `insert` function builds the Trie. Each node in the Trie corresponds to a letter in the words, and the `count` keeps track of how many words share this prefix.
- **Prefix Score Calculation:** The `getScore` function traverses the Trie and sums up the `count` values along the path from the root to the last character of the word, giving the prefix score.

*TC ->* O( `n * m` )
*SC ->* O( `n * m` )
where `n` is the number of words and `m` is the average length of the words.

```cpp title=Code
struct TrieNode {
    int count;
    TrieNode* children[26];

    TrieNode() {
        count = 0;
        for (int i = 0; i < 26; i++) {
            children[i] = nullptr;
        }
    }
};

class Solution {
    void insert(string word, TrieNode* root) {
        TrieNode* crawl = root;

        for (char ch : word) {
            int idx = ch - 'a';

            if (crawl->children[idx] == nullptr) {
                crawl->children[idx] = new TrieNode();
            }

            crawl = crawl->children[idx];
            crawl->count++;
        }
    }

    // returns sum of count of prefixes
    int getScore(string word, TrieNode* root) {
        TrieNode* crawl = root;

        int preCount = 0;

        for (char ch : word) {
            int idx = ch - 'a';
            if (crawl->children[idx] != nullptr) {
                crawl = crawl->children[idx];
                preCount += crawl->count;
            } else
                break;
        }

        return preCount;
    }

public:
    vector<int> sumPrefixScores(vector<string>& words) {
        TrieNode* root = new TrieNode();
        vector<int> res;
        for (string& word : words) {
            insert(word, root);
        }

        for (string& word : words) {
            res.push_back(getScore(word, root));
        }

        return res;
    }
};
```