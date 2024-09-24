---
difficulty: medium
leetcode-num: 3043
topics:
  - Hash Table
  - String
  - Trie
---
[Problem Link](https://leetcode.com/problems/find-the-length-of-the-longest-common-prefix/)

#### Problem
You are given two arrays with **positive** integers `arr1` and `arr2`.

A **prefix** of a positive integer is an integer formed by one or more of its digits, starting from its **leftmost** digit. For example, `123` is a prefix of the integer `12345`, while `234` is **not**.

A **common prefix** of two integers `a` and `b` is an integer `c`, such that `c` is a prefix of both `a` and `b`. For example, `5655359` and `56554` have a common prefix `565` while `1223` and `43456` **do not** have a common prefix.

You need to find the length of the **longest common prefix** between all pairs of integers `(x, y)` such that `x` belongs to `arr1` and `y` belongs to `arr2`.

Return _the length of the **longest** common prefix among all pairs_. _If no common prefix exists among them_, _return_ `0`.

**Example 1:**

**Input:** `arr1 = [1,10,100], arr2 = [1000]`
**Output:** 3
**Explanation:** There are 3 pairs `(arr1[i], arr2[j])`:
- The longest common prefix of (1, 1000) is 1.
- The longest common prefix of (10, 1000) is 10.
- The longest common prefix of (100, 1000) is 100.
The longest common prefix is 100 with a length of 3.

**Example 2:**

**Input:** `arr1 = [1,2,3], arr2 = [4,4,4]`
**Output:** 0
**Explanation:** There exists no common prefix for any pair `(arr1[i], arr2[j])`, hence we return 0.
Note that common prefixes between elements of the same array do not count.

**Constraints:**

- `1 <= arr1.length, arr2.length <= 5 * 104`
- `1 <= arr1[i], arr2[i] <= 108`

#### Solution
[Video Explanation](https://youtu.be/OZNS72LANFU)

##### Trie Approach
1. **TrieNode Structure**:
	- The `TrieNode` struct represents each node in the Trie.
	- Each node has an array of 10 pointers (`children[10]`), where each index corresponds to a digit (`0-9`).
	- The constructor initializes all elements of `children` to `nullptr`, indicating no children exist at the start.

2. **Insert Function**:

	- **Purpose**: To insert a number from `arr1` into the Trie.
	- The number is first converted to a string (`numStr`), allowing us to treat each digit individually.
	- For each digit in `numStr`, we compute its index (`idx = ch - '0'`), which maps the character to its corresponding integer value.
	- If the current node (`crawl`) does not have a child at that index (`nullptr`), we create a new `TrieNode`.
	- We then move (`crawl`) to the next node (child) and repeat this for all digits in the number.

3. **Search Function**:
	
	- **Purpose**: To find the length of the longest prefix of a number from `arr2` that matches any prefix in the Trie (built from `arr1`).
	- Similar to `insert`, the number is converted to a string (`numStr`).
	- For each digit, we check if the current node (`crawl`) has a child node at the index corresponding to that digit.
	- If a child exists, the prefix length is incremented (`length++`), and we move to the next node (child).
	- If a child does not exist, we stop, as the prefix no longer matches.

4. **Main Function (longestCommonPrefix)**:

	- A new `Trie` is created by initializing the root node (`root = new TrieNode()`).
	- All numbers in `arr1` are inserted into the Trie.
	- For each number in `arr2`, we search for the longest common prefix in the Trie using the `search` function and track the maximum prefix length across all numbers.
	- Finally, the length of the longest common prefix is returned.

_TC ->_ O( `(M + N) * D` )  
_SC ->_ O( `M * D` )
where `M` is the number of elements in `arr1`, `N` is the number of elements in `arr2` and `D` is the average number of digits per number.

```cpp title="Code"
struct TrieNode {
    TrieNode* children[10];

    TrieNode() {
        for (int i = 0; i < 10; i++) {
            children[i] = nullptr;
        }
    }
};

class Solution {
    void insert(int num, TrieNode* root) {
        TrieNode* crawl = root;
        string numStr = to_string(num);

        for (char ch : numStr) {
            int idx = ch - '0';

            if (crawl->children[idx] == nullptr) {
                crawl->children[idx] = new TrieNode();
            }

            crawl = crawl->children[idx];
        }
    }

    // returns length of longest prefix
    int search(int num, TrieNode* root) {
        TrieNode* crawl = root;
        string numStr = to_string(num);

        int length = 0;

        for (char ch : numStr) {
            int idx = ch - '0';
            if (crawl->children[idx] != nullptr) {
                length++;
                crawl = crawl->children[idx];
            } else
                break;
        }

        return length;
    }

public:
    int longestCommonPrefix(vector<int>& arr1, vector<int>& arr2) {
        TrieNode* root = new TrieNode();
        int longestPrefix = 0;

        for (int x : arr1) {
            insert(x, root);
        }

        for (int y : arr2) {
            longestPrefix = max(longestPrefix, search(y, root));
        }

        return longestPrefix;
    }
};
```

##### Hashing Approach
- **Initialization**:
    
    - An unordered set `st` is created to store prefixes from `arr1`.
    - `longestPrefix` is initialized to `0`, which will hold the length of the longest common prefix found.
- **Processing `arr1`**:
    
    - For each number `x` in `arr1`, the algorithm reduces the number digit by digit by dividing `x` by `10` (i.e., chopping off the last digit).
    - Each reduced number (including the full number) is inserted into the set `st` to represent all possible prefixes of `x`.
    
    For example, if `x = 123`, the values `123`, `12`, and `1` are inserted into the set.
    
- **Processing `arr2`**:
    
    - For each number `y` in `arr2`, the algorithm similarly reduces the number digit by digit, checking if any reduced form of `y` is present in `st` (i.e., already appeared as a prefix in `arr1`).
    - If a common prefix is found, the algorithm calculates the length of the common prefix using `log10(y) + 1` (this gives the number of digits in `y`), and updates `longestPrefix` to the maximum value found.
- **Final Step**:
    
    - Once both arrays have been processed, `longestPrefix` contains the length of the longest common prefix shared by elements of `arr1` and `arr2`.

*TC ->* O( `n * k` )
*SC ->* O( `n * k` )
where n is number of elements in input arrays, k is average number of digits in each element.

```cpp title=Code
class Solution {
public:
    int longestCommonPrefix(vector<int>& arr1, vector<int>& arr2) {
        unordered_set<int> st;
        int longestPrefix = 0;

        for (int& x : arr1) {
            while (!st.count(x) && x > 0) {
                st.insert(x);
                x /= 10;
            }
        }
        for (int& y : arr2) {
            while (!st.count(y) && y > 0) {
                y /= 10;
            }

            if (y > 0) {
                longestPrefix = max(longestPrefix, static_cast<int>(log10(y) + 1));
            }
        }

        return longestPrefix;
    }
};
```