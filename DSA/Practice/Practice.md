*Primary Topics*
%% Begin Waypoint %%
- **[[Arrays]]**
- **[[Backtracking]]**
- **[[Binary Trees]]**
- **[[Bit Manipulation]]**
- **[[Dynamic Programming]]**
- **[[Linked Lists]]**
- **[[Maths]]**
- **[[Strings]]**
- **[[Trie]]**

%% End Waypoint %%


*Named Algorithms*
- [[Longest Palindromic Substring (LC5)#Manacher's Algorithm|Manacher's Algorithm]]
- [[Maximum Sum Subarray - Kadane's Algo (LC53)|Kadane's Algorithm]]


#### Topic-wise Questions
##### Arrays

```dataview
TABLE WITHOUT ID file.link as Question, difficulty
from "DSA/Practice" or "Knowledge Base/DSA/Practice"
WHERE contains(topics,"Array") 
SORT choice(difficulty = "easy", "1", choice(difficulty = "medium", "2", choice(difficulty = "hard", "3", "other")))
```


##### Dynamic Programming
```dataview
TABLE WITHOUT ID file.link as Question, difficulty
from "DSA/Practice" or "Knowledge Base/DSA/Practice"
WHERE contains(topics,"Dynamic Programming") 
SORT choice(difficulty = "easy", "1", choice(difficulty = "medium", "2", choice(difficulty = "hard", "3", "other")))
```

##### Binary Trees

```dataview
TABLE WITHOUT ID file.link as Question, difficulty
from "DSA/Practice" or "Knowledge Base/DSA/Practice"
WHERE contains(topics,"Binary Tree") 
SORT choice(difficulty = "easy", "1", choice(difficulty = "medium", "2", choice(difficulty = "hard", "3", "other")))
```



#### Leetcode Problems

```dataview
TABLE WITHOUT ID leetcode-num as "Qn. No." , file.link as Question
from "DSA/Practice" or "Knowledge Base/DSA/Practice"
WHERE leetcode-num SORT leetcode-num
```