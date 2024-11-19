---
difficulty: medium
leetcode-num: 2491
topics:
  - Array
  - Hash Table
  - Two Pointer
  - Sorting
---
[Problem Link](https://leetcode.com/problems/divide-players-into-teams-of-equal-skill/)

#### Problem
You are given a positive integer array `skill` of **even** length `n` where `skill[i]` denotes the skill of the `ith` player. Divide the players into `n / 2` teams of size `2` such that the total skill of each team is **equal**.

The **chemistry** of a team is equal to the **product** of the skills of the players on that team.

Return _the sum of the **chemistry** of all the teams, or return_ `-1` _if there is no way to divide the players into teams such that the total skill of each team is equal._

**Example 1:**

**Input:** `skill = [3,2,5,1,3,4]`
**Output:** 22
**Explanation:** 
Divide the players into the following teams: (1, 5), (2, 4), (3, 3), where each team has a total skill of 6.
The sum of the chemistry of all the teams is: 1 * 5 + 2 * 4 + 3 * 3 = 5 + 8 + 9 = 22.

**Example 2:**

**Input:** `skill = [3,4]`
**Output:** 12
**Explanation:** 
The two players form a team with a total skill of 7.
The chemistry of the team is 3 * 4 = 12.

**Example 3:**

**Input:** skill = `[1,1,2,3]`
**Output:** -1
**Explanation:** 
There is no way to divide the players into teams such that the total skill of each team is equal.

**Constraints:**

- `2 <= skill.length <= 105`
- `skill.length` is even.
- `1 <= skill[i] <= 1000`

#### Solution
[Video Explanation]()

##### Hash Map Approach (Better Time Complexity)
1. **Count the frequency of skill values and compute the total sum**
    
    - A hash map (`unordered_map`) is used to store the frequency of each player's skill level.
    - At the same time, the total sum of all skill values is calculated, which helps in determining the required skill for each team.
2. **Calculate the target team skill**
    
    - The target skill for each team is calculated by dividing the total sum by half the size of the array (because each team will consist of 2 players).
    - This gives the skill level that each pair of players must sum to in order to form valid teams.
3. **Check for valid team formations**
    
    - For each skill level `x` in the map, the code looks for its complementary skill level `y` such that `x + y = teamSkill`.
        - If such a complementary skill exists, it means these players can form a valid team.
        - If no valid complement is found for a skill, the solution returns `-1` as it’s impossible to form valid teams.
4. **Calculate the total chemistry**
    
    - If valid teams are found, the chemistry for each team is calculated by multiplying `x * y * count`, where `x` and `y` are the skill levels of the players, and `count` is how many times that combination appears.
    - This ensures the total chemistry is properly accumulated for all teams.
5. **Final result**
    
    - The *total chemistry value is divided by 2* at the end to avoid double-counting pairs since each team is counted twice in the previous step.

This approach uses the hash map to efficiently check if the right pair of skills exists for each player and ensures the teams are formed correctly.

*TC ->* O( n )
*SC ->* O( n )

```cpp title=Code
class Solution {

public:
    long long dividePlayers(vector<int>& skill) {
        unordered_map<int, int> mp;
        long long sum = 0;

        for (int x : skill) {
            mp[x]++;
            sum += x;
        }

        long long teamSkill = (sum) / (skill.size() / 2);
        long long chemistry = 0;

        for (auto& [x, count] : mp) {
            int y = teamSkill - x;

            if (mp.count(y) && mp[y] == count) {
                chemistry += static_cast<long long>(x) * y * count;
            } else
                return -1;
        }

        return chemistry / 2;
    }
};
```

##### Two Pointer Approach (Better Space Complexity)
- **Sorting the Players by Skill**
    
    - First, the skill levels of all players are sorted in ascending order.
    - Sorting helps ensure that pairs can be easily formed by picking elements from opposite ends of the list.
- **Calculating the Total Skill of a Pair**
    
    - The sum of the first and last player's skill (smallest and largest skill) is taken as the target team skill.
    - This teamSkill will be the same for all pairs if the pairing is valid.
- **Forming Teams**
    
    - The algorithm iterates over the first half of the sorted list, pairing each player with their corresponding player from the second half of the list.
    - For each pair, the sum of their skills is checked against the target teamSkill.
    - If any pair’s skill sum doesn't match the teamSkill, it returns `-1` because a valid division of players isn't possible.
- **Calculating Chemistry**
    
    - If valid pairs are formed, the chemistry of each team (the product of the two players’ skill levels) is added to the total chemistry.
    - The chemistry is calculated as the product of the two skills, and this value is accumulated for all pairs.
- **Returning the Result**
    
    - After processing all pairs, the total chemistry is returned as the result.

*TC ->* O( `n * log n` )
*SC ->* O( 1 )

```cpp title=Code
class Solution {
public:
    long long dividePlayers(vector<int>& skill) {
        sort(skill.begin(), skill.end());

        int teamSkill = skill.front() + skill.back();
        long long chemistry = 0;

        for (int i = 0; i < skill.size() / 2; i++) {
            int x = skill[i];
            int y = skill[skill.size() - i - 1];

            if (x + y != teamSkill)
                return -1;

            chemistry += static_cast<long long>(x) * y;
        }

        return chemistry;
    }
};
```

##### Brute Force Approach
Make all possible pairs and calculate. This is very inefficient.
