---
tags:
    - Array
    - Binary Search
    - Sliding Window
    - Prefix Sum
    - Top Interviews
---

# [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

Given an array of positive integers `nums` and a positive integer `target`, return *the **minimal length** of a* 

subarray whose sum is greater than or equal to `target`. If there is no such subarray, return `0` instead.

**Example 1:**

```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
```

**Example 2:**

```
Input: target = 4, nums = [1,4,4]
Output: 1
```

**Example 3:**

```
Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0
```

 

**Constraints:**

- `1 <= target <= 109`
- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 104`

 

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution of which the time complexity is `O(n log(n))`.



**Solution:**

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int result = Integer.MAX_VALUE;

        int left = 0;
        int curSum = 0;
        

        for (int right = 0; right < nums.length; right++){
            curSum = curSum + nums[right];

            while(curSum >= target){
                result = Math.min(result, right - left + 1);
                curSum = curSum - nums[left];
                left++;
            }

        }

        if (result == Integer.MAX_VALUE){
            return 0;
        }else{
            return result;
        }

    }
}

// TC: O(n)
// SC: O(1)
```



```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int result = Integer.MAX_VALUE;

        int left = 0;
        int curSum = 0;
        

        for (int right = 0; right < nums.length; right++){
            curSum = curSum + nums[right];

            while(curSum - nums[left] >= target){
                curSum = curSum - nums[left];
                left++;
            }

            if (curSum >= target){
                result = Math.min(result, right - left + 1);
                // suppose right == left  -> 1 -> +1
            }

        }

        if (result == Integer.MAX_VALUE){
            return 0;
        }else{
            return result;
        }

    }
}

// TC: O(n)
// SC: O(1)
```



https://youtu.be/H8aKUOjPkEQ
