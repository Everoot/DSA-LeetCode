---
tag:
  - Array
  - Divide and Conquer
  - Dynamic Programming
  - Top Interviews
---

# [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

Given an integer array `nums`, find the subarray

with the largest sum, and return _its sum_.

**Example 1:**

```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: The subarray [4,-1,2,1] has the largest sum 6.
```

**Example 2:**

```
Input: nums = [1]
Output: 1
Explanation: The subarray [1] has the largest sum 1.
```

**Example 3:**

```
Input: nums = [5,4,-1,7,8]
Output: 23
Explanation: The subarray [5,4,-1,7,8] has the largest sum 23.
```

**Solution:**

Prefix:

```java
class Solution {
    public int maxSubArray(int[] nums) {
        // Assumptions: array != null && length >= 1
        // The subarray must contain at least one element.
        if (nums == null || nums.length == 0){
            return 0;
        }

        int result = nums[0];
        int sum = nums[0];

        // dp[i] means the largest sum of subarray ending at index i.
        // dp[i] = array[i]             if dp[i-1] <= 0
        //       = dp[i-1] + array[i].  if dp[i-1] > 0
        // So that we can reduce the space consumption by
        // recording the lastest sum.
        for (int i = 1; i < nums.length; i++){
            int cur = nums[i];


            sum = Math.max(sum + cur, cur);
            result = Math.max(sum, result);

        }

        return result;

    }
}

/*
nums = [-2,1,-3,4,-1,2,1,-5,4]
dp     [-2,1,-2,4,3, 5,6,1,5
        c
max = 6
*/

// TC:O(n)
// SC:O(1)
```

```java
class Solution {
    public int maxSubArray(int[] nums) {
        // base case
        int m = nums[0];
        int max = nums[0];

        for (int i = 1; i < nums.length; i++){
            // induction rule
            if (m < 0){
                m = nums[i];
            }else{ // m >= 0
                m = nums[i] + m;
            }

            // update max
            max = Math.max(m, max);
        }

        // return
        return max;
    }
}

// TC: O(n)
// SC: O(1)

/*
    index 0  1  2 3  4 5 6  7 8
    nums: -2,1,-3,4,-1,2,1,-5,4
    m[] : -2
             i

    m[0] = nums[-2];

    int global max =
    for (i =){
        if (m[i-1] < 0){
            m[i] = nums[i];
        }else{
            m[i] = nums[i] + m[i+1];
        }
        Max(m[i], max);
    }

    return max;




*/
```



dp:

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int[] dp = new int[nums.length]; // dp[i] = cur i max 

        int result = Integer.MIN_VALUE;

        for (int i = 0; i < nums.length; i++){
            if (i == 0){
                dp[i] = nums[i];
            }else{
                int pre = dp[i - 1];

                // inheritant
                int inheritant = dp[i - 1] + nums[i];
                // give up

                int cur = nums[i];

                dp[i] = Math.max(cur, inheritant);
            }

            result = Math.max(dp[i], result);
        }

        return result;
    }
}

// TC: O(n)
// SC: O(n)
```

