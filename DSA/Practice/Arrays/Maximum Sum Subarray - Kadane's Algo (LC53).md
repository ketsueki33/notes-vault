---
difficulty: medium
leetcode-num: 53
topics:
  - Array
  - Divide and Conquer
  - Dynamic Programming
---
**[Problem Link](https://leetcode.com/problems/maximum-subarray/)**
#### Problem
Given an integer array `nums`, find the subarray with the largest sum, and return _its sum_.

**Example 1:**

**Input:** nums =` [-2,1,-3,4,-1,2,1,-5,4]`
**Output:** 6
**Explanation:** The subarray `[4,-1,2,1]` has the largest sum 6.

**Example 2:**

**Input:** nums = `[1]`
**Output:** 1
**Explanation:** The subarray `[1] `has the largest sum 1.

**Example 3:**

**Input:** nums = `[5,4,-1,7,8]`
**Output:** 23
**Explanation:** The subarray `[5,4,-1,7,8]` has the largest sum 23.

**Constraints:**
- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

#### Solution
##### Optimal Approach ( Kadane's Algo )
[Article Explanation](https://www.simplilearn.com/kadanes-algorithm-article)

We will initialize two variables `max_ending_here` and `max_so_far` with the value of the first element from the given array.

> [!NOTE] 
>  If empty subarrays are allowed in the answer( i.e answer can be 0 if all array elements are negative) then initialize the two variables to `0` instead and start the following the loop from `0th` index.

`max_ending_here` will store the value of the maximum subarray sum ending at the current index.
`max_so_far` will store the value of the maximum subarray sum overall.

Traverse the given array starting from the second index ( `i=1`). At every point we will have two choices:-
- To include `nums[i]` in `max_ending_here`. We will choose this if adding `max_ending_here` gives a value greater than `nums[i]`
- To discard `max_ending_here` and start counting a new subarray sum from `i` with value `nums[i]`. We will choose this if `max_ending_here` is a negative value since including a negative value in the subarray will not give us the maximum subarray sum.

 `max_so_far` will be the maximum subarray sum.
 
![[Maximum Subarray - Kadane's Algo ( LC53 ) 2024-06-26 20.32.38.excalidraw|750]]

TC -> `O( n ) `
SC -> `O( 1 )`

```cpp title=Code
class Solution {
   public:
    int maxSubArray(vector<int>& nums) {
        int max_ending_here = nums[0];
        int max_so_far = nums[0];

        for (int i = 1; i < nums.size(); i++) {
            max_ending_here = max(max_ending_here + nums[i], nums[i]);
            max_so_far = max(max_ending_here, max_so_far);
        }

        return max_so_far;
    }
};
```

#### Variations
##### Print the subarray(s) with maximum sum
[Question Link](https://www.naukri.com/code360/problems/maximum-subarray_893296?leftPanelTabValue=PROBLEM)

There might be more than one subarray with the maximum sum. We need to print any of them.

**Intuition:** Our approach is to store the starting index and the ending index of the subarray. Thus we can easily get the subarray afterward without actually storing the subarray elements.

If we carefully observe the `max_ending_here`, we can notice that:
- maximum sum subarray always starts at the particular index where the previous `max_ending_here` is discarded. 
- Maximum sum subarray ends when the `max_ending_here`  crosses `max_so_far`

So, we will keep a track of the starting index inside the loop using a `curr_start` variable.

We will take two variables `ans_start` and `ans_end` initialized with 0. And when `max_ending_here` crosses  `max_so_far`, we will set `ans_start` to  `curr_start`variable and `ans_end` to the current index i.e. `i`.

The rest of the approach will be the same as Kadane’s Algorithm.

```cpp title=Code
vector<int> maximumsumSubarray(vector<int> &arr , int n)
{
    vector<int> res;
    int max_ending_here = arr[0];
    int max_so_far = arr[0];
    int curr_start = 0;
    int ans_start = 0;
    int ans_end = 0;

    for( int i = 1 ; i < n ; i++ ){
        if( max_ending_here < 0 ){
            curr_start = i;
            max_ending_here = arr[i];
        }
        else max_ending_here = arr[i] + max_ending_here;

        if(max_ending_here > max_so_far){
            ans_start = curr_start;
            ans_end = i;
            max_so_far = max_ending_here;
        }
    }
    for( int i = ans_start; i <= ans_end ; i++ )
        res.push_back(arr[i]);
    return res;
}
```

##### Maximum Sum Subarray with Divide and Conquer
[Video Explanation](https://youtu.be/3GD-3UZGsVI)

*below answer taken from [link](https://leetcode.com/problems/maximum-subarray/solutions/1595195/c-python-7-simple-solutions-w-explanation-brute-force-dp-kadane-divide-conquer)*

We can solve this using divide and conquer strategy. This is the follow-up question asked in this question. This involves modelling the problem by observing that the maximum subarray sum is such that it lies somewhere -

- entirely in the left-half of array `[L, mid-1]`, OR
- entirely in the right-half of array `[mid+1, R]`, OR
- in array consisting of mid element along with some part of left-half and some part of right-half such that these form contiguous subarray - `[L', R'] = [L', mid-1] + [mid] + [mid+1,R']`, where `L' >= L` and `R' <= R`

With the above observation, we can recursively divide the array into sub-problems on the left and right halves and then combine these results on the way back up to find the maximum subarray sum.

```cpp title=Code
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        return maxSubArray(nums, 0, size(nums)-1);
    }
    int maxSubArray(vector<int>& A, int L, int R){
        if(L > R) return INT_MIN;
        int mid = (L + R) / 2, leftSum = 0, rightSum = 0;
        // leftSum = max subarray sum in [L, mid-1] and starting from mid-1
        for(int i = mid-1, curSum = 0; i >= L; i--)
            curSum += A[i],
            leftSum=max(leftSum, curSum);
        // rightSum = max subarray sum in [mid+1, R] and starting from mid+1
        for(int i = mid+1, curSum = 0; i <= R; i++)
            curSum += A[i],
            rightSum = max(rightSum, curSum);        
		// return max of 3 cases 
        return max({ maxSubArray(A, L, mid-1), maxSubArray(A, mid+1, R), leftSum + A[mid] + rightSum });
    }	
};
```