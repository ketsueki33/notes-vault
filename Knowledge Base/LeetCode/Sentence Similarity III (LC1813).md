---
difficulty: medium
leetcode-num: 1813
topics:
  - String
  - Array
  - Two Pointer
---
[Problem Link](https://leetcode.com/problems/sentence-similarity-iii/)

#### Problem
You are given two strings `sentence1` and `sentence2`, each representing a **sentence** composed of words. A sentence is a list of **words** that are separated by a **single** space with no leading or trailing spaces. Each word consists of only uppercase and lowercase English characters.

Two sentences `s1` and `s2` are considered **similar** if it is possible to insert an arbitrary sentence (_possibly empty_) inside one of these sentences such that the two sentences become equal. **Note** that the inserted sentence must be separated from existing words by spaces.

For example,

- `s1 = "Hello Jane"` and `s2 = "Hello my name is Jane"` can be made equal by inserting `"my name is"` between `"Hello"` and `"Jane"` in s1.
- `s1 = "Frog cool"` and `s2 = "Frogs are cool"` are **not** similar, since although there is a sentence `"s are"` inserted into `s1`, it is not separated from `"Frog"` by a space.

Given two sentences `sentence1` and `sentence2`, return **true** if `sentence1` and `sentence2` are **similar**. Otherwise, return **false**.

**Example 1:**

**Input:** sentence1 = "My name is Haley", sentence2 = "My Haley"

**Output:** true

**Explanation:**

`sentence2` can be turned to `sentence1` by inserting "name is" between "My" and "Haley".

**Example 2:**

**Input:** sentence1 = "of", sentence2 = "A lot of words"

**Output:** false

**Explanation:**

No single sentence can be inserted inside one of the sentences to make it equal to the other.

**Example 3:**

**Input:** sentence1 = "Eating right now", sentence2 = "Eating"

**Output:** true

**Explanation:**

`sentence2` can be turned to `sentence1` by inserting "right now" at the end of the sentence.

**Constraints:**

- `1 <= sentence1.length, sentence2.length <= 100`
- `sentence1` and `sentence2` consist of lowercase and uppercase English letters and spaces.
- The words in `sentence1` and `sentence2` are separated by a single space.

#### Solution
[Video Explanation](https://youtu.be/J9KwcuukMZE)

##### Dequeue Approach (Simpler)
- **Handling Sentence Sizes**:
    
    - The function first ensures that `s1` is always the longer sentence (or equal in length). If `s1` is shorter than `s2`, we swap them. This simplifies the comparison since we will always check if we can transform the longer sentence into the shorter one.
- **Tokenization of Sentences**:
    
    - The sentences `s1` and `s2` are split into individual words using `stringstream`. These words are stored in two **deques** (`dq1` for `s1` and `dq2` for `s2`), where each word is pushed either at the front or back.
    - **Deque** is used because it allows easy removal of elements from both the front and back, which is key to this approach.
- **Front Matching**:
    
    - The first `while` loop compares the words from the **front** of both deques (`dq1` and `dq2`). If the words at the front of both sentences are the same, we pop (remove) them from both deques.
    - This continues until either the front words stop matching or one of the deques is empty.
- **Back Matching**:
    
    - Similarly, the second `while` loop compares words from the **back** of both deques. As long as the words at the back of both sentences match, we remove them.
    - This continues until there are no more matching words at the back or one of the deques is empty.
- **Final Check**:
    
    - After the front and back matching, the key condition is whether `dq2` is empty:
        - If `dq2` is empty, it means all the words from the shorter sentence `s2` were successfully matched with either the beginning or end (or both) of `s1`.
        - If `dq2` is not empty, it means there are still words in `s2` that could not be matched with `s1`, so the sentences are not similar.

*TC ->* O( m + n )
*SC ->* O( m + n )

```cpp title=Code
class Solution {
public:
    bool areSentencesSimilar(string s1, string s2) {
        if (s1.size() < s2.size())
            swap(s1, s2);

        stringstream ss1(s1), ss2(s2);

        deque<string> dq1, dq2;
        string token;
        while (ss1 >> token) {
            dq1.push_back(token);
        }
        while (ss2 >> token) {
            dq2.push_back(token);
        }

        while (!dq1.empty() && !dq2.empty() && dq1.front() == dq2.front()) {
            dq1.pop_front();
            dq2.pop_front();
        }

        while (!dq1.empty() && !dq2.empty() && dq1.back() == dq2.back()) {
            dq1.pop_back();
            dq2.pop_back();
        }

        return dq2.empty();
    }
};
```


##### Two Pointer with Vectors Approach
- **String Size Comparison**:
    
    - If one string is shorter than the other, it’s beneficial to treat the longer one as `s1` and the shorter one as `s2`. This ensures we are always checking whether `s2` is a "subsequence" of `s1` by adding words at the beginning or end of `s1`.
- **Tokenization of Sentences**:
    
    - We split both strings (`s1` and `s2`) into individual words.
    - This is done using `stringstream` to extract each word and store them in separate vectors `v1` and `v2`.
- **Two-Pointer Approach**:
    
    - The two-pointer technique is applied to check for word matches from both the start and the end of the sentences.
    - **Pointers**:
        - `i` starts from the beginning of `v1` (longer sentence).
        - `j` starts from the end of `v1`.
        - `k` starts from the beginning of `v2` (shorter sentence).
        - `l` starts from the end of `v2`.
    - The goal is to match words from the beginning (`i` and `k`) and from the end (`j` and `l`).
- **Matching Words**:
    
    - Start from the **beginning** of both vectors (`v1` and `v2`) and compare the words:
        - As long as words match (`v1[i] == v2[k]`), move the `i` and `k` pointers forward.
    - Next, start from the **end** of both vectors and compare:
        - As long as words match (`v1[j] == v2[l]`), move the `j` and `l` pointers backward.
- **Final Check**:
    
    - After processing, if all words in `v2` are matched either at the beginning or end of `v1`, then the remaining part of `v1` (middle section) doesn't need to match. In other words, `v2` should be fully consumed:
        - If `l < k` after the loops, it means the entire shorter sentence `v2` was matched, and the two sentences are considered similar.

*TC ->* O( m + n )
*SC ->* O( m + n )

```cpp title=Code
class Solution {
public:
    bool areSentencesSimilar(string s1, string s2) {
        if (s1.size() < s2.size())
            swap(s1, s2);

        stringstream ss1(s1), ss2(s2);

        vector<string> v1, v2;
        string token;
        while (ss1 >> token) {
            v1.push_back(token);
        }
        while (ss2 >> token) {
            v2.push_back(token);
        }

        int i = 0, j = v1.size() - 1;
        int k = 0, l = v2.size() - 1;

        while (k < v2.size() && i < v1.size() && v1[i] == v2[k]) {
            i++;
            k++;
        }
        while (l >= k && v1[j] == v2[l]) {
            j--;
            l--;
        }
        
        return l < k;
    }
};
```