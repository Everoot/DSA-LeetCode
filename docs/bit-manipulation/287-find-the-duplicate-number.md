---
tags:
  - Array
  - Two Pointers
  - Binary Search
  - Bit Manipulation
  - Top Interviews
---

# [287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one repeated number** in `nums`, return _this repeated number_.

You must solve the problem **without** modifying the array `nums` and uses only constant extra space.

**Example 1:**

```
Input: nums = [1,3,4,2,2]
Output: 2
```

**Example 2:**

```
Input: nums = [3,1,3,4,2]
Output: 3
```

**Example 3:**

```
Input: nums = [3,3,3,3,3]
Output: 3
```

**Constraints:**

- `1 <= n <= 105`
- `nums.length == n + 1`
- `1 <= nums[i] <= n`
- All the integers in `nums` appear only **once** except for **precisely one integer** which appears **two or more** times.

**Follow up:**

- How can we prove that at least one duplicate number must exist in `nums`?
- Can you solve the problem in linear runtime complexity?



**Solution**:

Floyd's Tortoise and Hare (Cycle Detection)

Imagine there is $n + 1$. A graph with nodes, node numbers starting from $0$ to $n$.

For $i = 0, 1, 2, ..., n$ , 

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = 0;
        int fast = 0;

        slow = nums[slow];
        fast = nums[nums[fast]];


        // Phase 1: Find the intersection point in the cycle
        while (slow != fast){
            slow = nums[slow];
            fast = nums[nums[fast]];
        }

        // Phase 2: Find the entrance to the cycle

        slow = 0;
        while(slow != fast){
            slow = nums[slow];
            fast = nums[fast];
        }

        return slow;
    }
}

// TC: O(n)
// SC: O(1)
```

```java
class Solution {
    public int findDuplicate(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();

        for (int i = 0; i < nums.length; i++){
            if (set.contains(nums[i])){
                return nums[i];
            }else{
                set.add(nums[i]);
            }
        }
        return -1;
    }
}

// TC: O(n)
// SC: O(n)
```
