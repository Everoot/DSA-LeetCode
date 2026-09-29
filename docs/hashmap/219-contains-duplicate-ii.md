---
tags:
    - Array
    - Hash Table
    - Sliding Window
    - Top Interview 150
---

# [219. Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)

Given an integer array `nums` and an integer `k`, return `true` *if there are two **distinct indices*** `i` *and* `j` *in the array such that* `nums[i] == nums[j]` *and* `abs(i - j) <= k`. 

**Example 1:**

```
Input: nums = [1,2,3,1], k = 3
Output: true
```

**Example 2:**

```
Input: nums = [1,0,1,1], k = 1
Output: true
```

**Example 3:**

```
Input: nums = [1,2,3,1,2,3], k = 2
Output: false
```



**Solution:**

```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++){
            int cur = nums[i];
            if (map.containsKey(cur)){
                int index = map.get(cur);
                int abs = Math.abs(index - i);
                if (abs <= k){
                    return true;
                }
            }
            map.put(cur, i);
        }

        return false;


    }
}

// TC: O(n)
// SC: O(n)
```



```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Set<Integer> set = new HashSet<>();
        for (int i = 0; i < nums.length; i++){
            if (set.contains(nums[i])){
                return true;
            }
            set.add(nums[i]);
            if (set.size() > k){
                set.remove(nums[i-k]);
            }
        }
        return false;
    }
}
// TC:O(n)
// SC:O(min(n, k))
```

