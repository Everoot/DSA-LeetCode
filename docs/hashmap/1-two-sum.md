---
tags: 
    - Array
    - Hash Table
comments: true
---

# [1. Two Sum](https://leetcode.com/problems/two-sum/) 

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.

You may assume that each input would have **exactly one solution**, and you may not use the *same* element twice.

You can return the answer in any order.

**Example 1:**

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

**Example 2:**

```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

**Example 3:**

```
Input: nums = [3,3], target = 6
Output: [0,1]
```



**Solution:**

**Approach 1: Brute Force**

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] result = new int[2];
        for (int i = 0; i < nums.length; i++){
            for(int j = i + 1; j < nums.length; j++){
                if (nums[i] + nums[j] == target){
                    result[0] = i;
                    result[1] = j;
                    break;
                }
            }
        }

        return result;
    }
}

// TC: O(n^2)
// SC: O(n)
```



**Approach 2: Two-pass Hash Table**

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] result = new int[2];
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        // value , index

        for (int i = 0; i < nums.length; i++){
            int cur = nums[i];
            if (map.containsKey(target - cur)){
                result[0] = map.get(target - cur);
                result[1] = i;
                return result;
            }
            map.put(cur, i);
        }

        return result;
    }
}

// TC: O(n)
// SC: O(n)
```



```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> map = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; i++){
            int m = target - nums[i];
            if (map.containsKey(m)){
                return new int[]{map.get(m),  i};
            }
            map.put(nums[i], i);
        }
        return new int[]{};
    }
}
// TC: O(n)
// SC: O(n)
```



```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++){
            for (int j = i+1; j < nums.length; j++){
                if( (nums[i] + nums[j]) == target){
                    return new int[]{i,j};
                }
            }
        }
        return null;
        
    }
}
// TC:O(n^2)
// SC:O(1)
```





