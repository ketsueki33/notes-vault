---
difficulty: medium
leetcode-num: 3011
topics:
  - Array
  - Bit Manipulation
  - Sorting
---
[Problem Link](https://leetcode.com/problems/find-if-array-can-be-sorted/)

#### Problem
You are given a **0-indexed** array of **positive** integers `nums`.

In one **operation**, you can swap any two **adjacent** elements if they have the **same** number of

set bits

. You are allowed to do this operation **any** number of times (**including zero**).

Return `true` _if you can sort the array, else return_ `false`.

**Example 1:**

**Input:** nums = [8,4,2,30,15]
**Output:** true
**Explanation:** Let's look at the binary representation of every element. The numbers 2, 4, and 8 have one set bit each with binary representation "10", "100", and "1000" respectively. The numbers 15 and 30 have four set bits each with binary representation "1111" and "11110".
We can sort the array using 4 operations:
- Swap nums[0] with nums[1]. This operation is valid because 8 and 4 have one set bit each. The array becomes [4,8,2,30,15].
- Swap nums[1] with nums[2]. This operation is valid because 8 and 2 have one set bit each. The array becomes [4,2,8,30,15].
- Swap nums[0] with nums[1]. This operation is valid because 4 and 2 have one set bit each. The array becomes [2,4,8,30,15].
- Swap nums[3] with nums[4]. This operation is valid because 30 and 15 have four set bits each. The array becomes [2,4,8,15,30].
The array has become sorted, hence we return true.
Note that there may be other sequences of operations which also sort the array.

**Example 2:**

**Input:** nums = [1,2,3,4,5]
**Output:** true
**Explanation:** The array is already sorted, hence we return true.

**Example 3:**

**Input:** nums = [3,16,8,4,2]
**Output:** false
**Explanation:** It can be shown that it is not possible to sort the input array using any number of operations.

**Constraints:**

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 28`
#### Solution
[Video Explanation](https://youtu.be/TYOnpelfejY)

##### Optimal Approach
###### Key Functions and Variables

1. **`setBits(int num)` Function**:
    
    - This helper function counts the number of `1`s in the binary representation of `num`.
    - `bitCount` is initialized to 0.
    - The function iterates while `num` is non-zero, incrementing `bitCount` each time the least significant bit of `num` is `1`.
    - After checking, `num` is right-shifted by 1 bit.
    - It returns the total count of `1`s, which represents the number of set bits in `num`.
2. **`canSortArray(vector<int>& nums)` Function**:
    
    - This function iterates over `nums` and checks whether `nums` can be sorted based on groups defined by set bit counts.
    
    Key variables:
    
    - `prevMax`: Tracks the maximum element of the previous group.
    - `currMax` and `currMin`: Track the max and min values within the current group of elements (with the same number of set bits).
    - `currBits`: Stores the set bit count of the current group of elements.

###### Step-by-Step Logic

1. **Iterate Through the Array**:
    
    - Start with `i = 0`, and iterate while `i < n`, where `n` is the size of `nums`.
2. **Identify Groups of Elements with the Same Set Bit Count**:
    
    - For each element at `nums[i]`, calculate its set bit count using `setBits(nums[i])`.
    - Start a new group with this count (`currBits`) and initialize `currMax` to `INT_MIN` and `currMin` to `INT_MAX` to track the maximum and minimum values within this group.
3. **Process Each Group**:
    
    - Continue iterating through `nums` while the set bit count of `nums[i]` matches `currBits`.
    - For each element in this group:
        - Update `currMax` to be the maximum of the current `currMax` and `nums[i]`.
        - Update `currMin` to be the minimum of the current `currMin` and `nums[i]`.
    - After processing all elements with the same set bit count, `i` will point to the start of the next group (with a different set bit count).
4. **Check Group Order**:
    
    - After each group, compare `currMin` (the minimum element in the current group) with `prevMax` (the maximum element in the previous group).
    - If `currMin < prevMax`, the groups overlap in a way that would make sorting impossible, so return `false`.
    - Update `prevMax` to be the `currMax` of the current group before moving to the next group.
5. **Final Result**:
    
    - If all groups satisfy the condition (`currMin >= prevMax`), return `true`, indicating that sorting is possible.

*TC ->* O( n ), if we consider `setBits` function to be O(1)
*SC ->* O( 1 )

```cpp title=Code
class Solution {
    int setBits(int num) {
        int bitCount = 0;
        while (num) {
            if ((num & 1) == 1)
                bitCount++;
            num >>= 1;
        }

        return bitCount;
    }

public:
    bool canSortArray(vector<int>& nums) {
        int n = nums.size();

        int prevMax = -1;
        int i = 0;

        while (i < n) {
            int currMax = INT_MIN;
            int currMin = INT_MAX;
            int currBits = setBits(nums[i]);
            while (i < n && setBits(nums[i]) == currBits) {
                currMax = max(currMax, nums[i]);
                currMin = min(currMin, nums[i]);
                i++;
            }

            if (currMin < prevMax)
                return false;

            prevMax = currMax;
        }

        return true;
    }
};
```

##### Bubble Sort - Alright Approach
- **Generating the `bits` Vector**:
    
    - This vector stores the set bit count for each number in `nums`, calculated using the `setBits` function.
    - For each number `x` in `nums`, `bits.push_back(setBits(x))` appends its set bit count to the `bits` array.
- **Bubble Sort with Condition**:
    
    - The algorithm attempts to sort `nums` using a modified version of Bubble Sort.
        
    - `flag` is used to track whether any swaps were made during a pass. If no swaps are made, `flag` remains `false`, ending the loop.
        
    - **Loop and Swap Condition**:
        
        - It iterates through `nums` comparing adjacent elements: `nums[i]` and `nums[i + 1]`.
        - If `nums[i] > nums[i + 1]`, indicating they’re out of order, it checks if `bits[i] == bits[i + 1]`.
            - If `bits[i] != bits[i + 1]`, it returns `false` immediately because the adjacent elements cannot be swapped.
            - If `bits[i] == bits[i + 1]`, it performs a swap on `nums[i]` and `nums[i + 1]` and sets `flag = true` to indicate a change was made.
- **Return Result**:
    
    - If the loop completes without returning `false`, it means `nums` was successfully sorted according to the set bit constraint, and the function returns `true`.

*TC ->* O( n$^{2}$ )
*SC ->* O( n )

```cpp title=Code
class Solution {
    int setBits(int num) {
        int bitCount = 0;
        while (num) {
            if ((num & 1) == 1)
                bitCount++;
            num >>= 1;
        }

        return bitCount;
    }

public:
    bool canSortArray(vector<int>& nums) {
        vector<int> bits;

        for (int x : nums)
            bits.push_back(setBits(x));

        bool flag = true;

        while (flag) {
            flag = false;
            for (int i = 0; i < nums.size() - 1; i++) {
                if (nums[i] > nums[i + 1]) {
                    if (bits[i] != bits[i + 1])
                        return false;
                    swap(nums[i], nums[i + 1]);
                    flag = true;
                }
            }
        }

        return true;
    }
};
```