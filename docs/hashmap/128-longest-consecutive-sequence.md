---
tags:
    - Array
    - Hash Table
    - Union Find
    - Top Interveiws
---

# [128. Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

Given an unsorted array of integers `nums`, return *the length of the longest consecutive elements sequence.*

You must write an algorithm that runs in `O(n)` time.

**Example 1:**

```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

**Example 2:**

```
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9
```

**Solution:**

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums == null || nums.length == 0){
            return 0;
        }
        Arrays.sort(nums);
        int result = 1;
        int count = 1;
        for (int i = 1; i < nums.length; i++){
            if (nums[i] == nums[i - 1]){
                continue;
            }else if (nums[i] == nums[i-1] + 1){
                count = count + 1;
            }else{
                count = 1;
            }
            result = Math.max(result, count);
        }
        return result;
    }
}
// TC: O(nlogn)
// SC: O(1)
```



```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums == null || nums.length == 0){
            return 0;
        }

        Set<Integer> set = new HashSet<Integer>();
        for (int i = 0; i < nums.length; i++){
            set.add(nums[i]);
            
        }

        int result = 1;
        int count = 1;
        for (int num : set){
            if (!set.contains(num - 1)){
                int cur = num;
                count = 1;

                while(set.contains(cur + 1)){
                    count = count + 1;
                    cur = cur + 1;
                }
            }

            result = Math.max(count, result);
        }

        return result;
    }
}

// TC: O(n)
// SC: O(n)
```

