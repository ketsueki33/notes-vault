---
difficulty: hard
leetcode-num: 432
topics:
  - Hash Table
  - Design
  - Linked List
  - Doubly-Linked List
---
[Problem Link](https://leetcode.com/problems/all-oone-data-structure/)

#### Problem
Design a data structure to store the strings' count with the ability to return the strings with minimum and maximum counts.

Implement the `AllOne` class:

- `AllOne()` Initializes the object of the data structure.
- `inc(String key)` Increments the count of the string `key` by `1`. If `key` does not exist in the data structure, insert it with count `1`.
- `dec(String key)` Decrements the count of the string `key` by `1`. If the count of `key` is `0` after the decrement, remove it from the data structure. It is guaranteed that `key` exists in the data structure before the decrement.
- `getMaxKey()` Returns one of the keys with the maximal count. If no element exists, return an empty string `""`.
- `getMinKey()` Returns one of the keys with the minimum count. If no element exists, return an empty string `""`.

**Note** that each function must run in `O(1)` average time complexity.

**Example 1:**

**Input**
`["AllOne", "inc", "inc", "getMaxKey", "getMinKey", "inc", "getMaxKey", "getMinKey"]`
`[[], ["hello"], ["hello"], [], [], ["leet"], [], []]`
**Output**
`[null, null, null, "hello", "hello", null, "hello", "leet"]`

**Explanation**
AllOne allOne = new AllOne();
allOne.inc("hello");
allOne.inc("hello");
allOne.getMaxKey(); // return "hello"
allOne.getMinKey(); // return "hello"
allOne.inc("leet");
allOne.getMaxKey(); // return "hello"
allOne.getMinKey(); // return "leet"

**Constraints:**

- `1 <= key.length <= 10`
- `key` consists of lowercase English letters.
- It is guaranteed that for each call to `dec`, `key` is existing in the data structure.
- At most `5 * 104` calls will be made to `inc`, `dec`, `getMaxKey`, and `getMinKey`.

#### Solution
[Video Explanation](https://youtu.be/GJJ2OWKHADs)

##### Optimal Approach
###### Key Concepts

- **Node Structure:**  
    Each node represents a unique count and holds a set of keys with that count. For example, a node with `count = 2` will hold all keys that have been incremented exactly twice. The nodes are linked together in a doubly linked list, allowing traversal between counts in both directions.
    
- **Doubly Linked List:**
    
    - `dHead`: A dummy head node with a count of `0`. It helps in handling edge cases like incrementing keys from 0.
    - `dTail`: A dummy tail node with a count of `-1`. It marks the end of the list.
    
    Nodes are dynamically added or removed between the head and tail based on key counts.
    
- **HashMap (`mp`):**  
    Maps each key (string) to the corresponding node in the doubly linked list. This enables O(1) access to a key's current node, allowing for efficient increments and decrements.
    

###### Methods Explanation:

1. **Constructor (AllOne):**
    
    - Initializes the doubly linked list with `dHead` and `dTail`.
    - Sets `dHead`'s next pointer to `dTail` and `dTail`'s previous pointer to `dHead`.
2. **Increment (`inc`):**
    
    - If the key exists, it is removed from its current node (`ogNode`), and the count is increased by 1.
    - A new node for the new count is inserted after the current node if it doesn't exist.
    - The key is added to this new node's set.
    - If the old node has no more keys after removal, the node is deleted.
3. **Decrement (`dec`):**
    
    - If the key exists, it is removed from its current node (`ogNode`), and the count is decreased by 1.
    - If the count reaches `0`, the key is removed from the map.
    - If there is no node for the new count, a new node is inserted before the current one.
    - The key is added to the node with the new count. If the old node is empty after removal, it is deleted.
4. **Get Maximum Key (`getMaxKey`):**
    
    - Returns any key from the node just before `dTail`, which has the highest count. If the list is empty (only dummy nodes exist), it returns an empty string.
5. **Get Minimum Key (`getMinKey`):**
    
    - Returns any key from the node just after `dHead`, which has the smallest count. If no valid nodes exist, it returns an empty string.

###### Helper Methods:

- **`insertNode(prev, c)`:** Inserts a new node with count `c` between `prev` and `prev->next`.
- **`removeNode(tbr)`:** Removes a node `tbr` from the list and re-links its neighbors.

*TC ->* O( 1 ) for each function
*SC ->* O( n )

```cpp title=Code
struct Node {
    int count;
    unordered_set<string> keys;
    Node* prev;
    Node* next;

    Node(int c) {
        count = c;
        prev = nullptr;
        next = nullptr;
    }
};

class AllOne {
    unordered_map<string, Node*> mp;
    Node* dHead;
    Node* dTail;

    Node* insertNode(Node* prev, int c) {
        Node* next = prev->next;
        Node* temp = new Node(c);

        prev->next = temp;
        next->prev = temp;
        temp->prev = prev;
        temp->next = next;

        return temp;
    }

    void removeNode(Node* tbr) {
        Node* prev = tbr->prev;
        Node* next = tbr->next;

        prev->next = next;
        next->prev = prev;

        delete tbr;
    }

public:
    AllOne() {
        dHead = new Node(0);
        dTail = new Node(-1);
        dHead->next = dTail;
        dTail->prev = dHead;
    }

    void inc(string key) {
        Node* ogNode = dHead;

        if (mp.count(key)) {
            ogNode = mp[key];
            ogNode->keys.erase(key);
        }

        Node* currNode = ogNode->next;

        if (currNode->count != ogNode->count + 1)
            currNode = insertNode(ogNode, ogNode->count + 1);

        currNode->keys.insert(key);
        mp[key] = currNode;

        if (ogNode != dHead && ogNode->keys.empty())
            removeNode(ogNode);
    }

    void dec(string key) {
        if (mp.find(key) == mp.end())
            return; // Add this check

        Node* ogNode = mp[key];

        ogNode->keys.erase(key);

        Node* currNode = ogNode->prev;

        if (currNode->count != ogNode->count - 1) {
            currNode = insertNode(currNode, ogNode->count - 1);
        }

        if (currNode != dHead) {
            currNode->keys.insert(key);
            mp[key] = currNode;
        } else {
            mp.erase(key);
        }

        if (ogNode->keys.empty())
            removeNode(ogNode);
    }

    string getMaxKey() {
        Node* target = dTail->prev;

        if (target == dHead)
            return "";
        return *target->keys.begin();
    }

    string getMinKey() {
        Node* target = dHead->next;

        if (target == dTail)
            return "";
        return *target->keys.begin();
    }
};
```