---
tags:
    - Array
    - Dynamic Programming
---

# [213. House Robber II](https://leetcode.com/problems/house-robber-ii/)

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

 

**Example 1:**

```
Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
```

**Example 2:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Example 3:**

```
Input: nums = [1,2,3]
Output: 3
```

 

**Constraints:**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 1000`



**Solution:**

```java
class Solution {
    public int rob(int[] nums) {
        // base case 
        if (nums == null || nums.length == 0){
            return 0;
        }

        if (nums.length == 1 ){
            return nums[0];
        }

        if (nums.length == 2){
            return Math.max(nums[0], nums[1]);
        }




        int start1 = 0;
        int end1 = nums.length - 2;
        int start2 = 1;
        int end2= nums.length -1;

        int rob1 = helper(start1, end1, nums);
        int rob2 = helper(start2, end2, nums);

        return Math.max(rob1, rob2);

    }

    private int helper(int start, int end, int[] nums){
       //  [1,2,3,     1]
       //       i
       //     f
       //   s
        int[] dp = new int[nums.length];
        dp[start] = nums[start];
        dp[start+1] = Math.max(nums[start+1], nums[start]); 
        for (int i = start + 2; i <= end; i++){

            dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i]);

        }

        return dp[end];
    }
}

// TC: O(n)
// SC: O(n)
```



```java
class Solution {
    public int rob(int[] nums) {
        // base case 
        if (nums == null || nums.length == 0){
            return 0;
        }

        if (nums.length == 1 ){
            return nums[0];
        }

        if (nums.length == 2){
            return Math.max(nums[0], nums[1]);
        }

        int start1 = 0;
        int end1 = nums.length - 2;
        int start2 = 1;
        int end2= nums.length -1;

        int rob1 = helper(start1, end1, nums);
        int rob2 = helper(start2, end2, nums);

        return Math.max(rob1, rob2);

    }

    private int helper(int start, int end, int[] nums){
        int fast = 0;
        int slow = 0;
        for (int i = start; i <= end; i++){
            int temp = fast; // 0 // 1// 2 
            int cur = nums[i];// 1 // 2  // 3
            // update fast
            fast = Math.max(temp, slow + cur);/// Math.(0, 1)
             /// (1, 0+2) = 2 // 1 + 3 = 4 , 2
            slow = temp;//  2 

        }
        
        return dp[end];
    }
}

// TC: O(n)
// SC: O(1)
```

