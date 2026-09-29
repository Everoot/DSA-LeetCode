---
tags:
    - Array
    - Binary Search
    - Top Interviews
---



# [162. Find Peak Element](https://leetcode.com/problems/find-peak-element/)

A peak element is an element that is strictly greater than its neighbors.

Given a **0-indexed** integer array `nums`, find a peak element, and return its index. If the array contains multiple peaks, return the index to **any of the peaks**.

You may imagine that `nums[-1] = nums[n] = -∞`. In other words, an element is always considered to be strictly greater than a neighbor that is outside the array.

You must write an algorithm that runs in `O(log n)` time.

 

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```

**Example 2:**

```
Input: nums = [1,2,1,3,5,6,4]
Output: 5
Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.
```

 

**Constraints:**

- `1 <= nums.length <= 1000`
- `-231 <= nums[i] <= 231 - 1`
- `nums[i] != nums[i + 1]` for all valid `i`.





Solution:

brute force

```java
class Solution {
    public int findPeakElement(int[] nums) {
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] > nums[i + 1])
                return i;
        }
        return nums.length - 1;
    }
}
// TC: O(n)
// SC: O(1)
```



binarySearch:

```java
class Solution {
    public int findPeakElement(int[] nums) {
       int left = 0;
       int right = nums.length -1 -1;
       // 未知搜索范围 [0, nums.length - 1]
       // 因为要判断mid+1
       // 如果right为nums.length-1则可能会产生数组越界问题
       // 最后一个数可以是峰值，比如 nums=[5,6]，答案是 1

       while(left <= right){
        int mid = left + (right - left)/2;
        if (nums[mid] > nums[mid + 1] ){
            right = mid - 1;
        }else{
            left = mid + 1; 
        }
       } 

       return left;
    }
}

// TC: O(logn)
// SC: O(1)
```



```java
class Solution {
    public int findPeakElement(int[] nums) {
        // 0, 0-2
        // [0, n-1]
        //  [1,2,3,1]
        //   i     r
        //     m
        //  
        // [0, m] [m + 1, r]
        int n = nums.length;
        int left = 0;
        int right = nums.length - 1;
        
        while(left < right){
            int mid = left + (right - left)/2;

            if (nums[mid] < nums[mid + 1]){
                left = mid + 1;
            }else{
                right = mid;
            }
        }
        return left;
        
    }
}

// TC: O(logn)
// SC: O(1)
```



```java
class Solution {
    public int findPeakElement(int[] nums) {
        int n = nums.length;

        int left = 0;
        int right = n - 1; 
        // [0, m]  // [m + 1, n - 1]
        // <= peak    < peak
        // -> left
        // [left, right] 
        // 
        while(left < right){
            int mid = left + (right - left)/ 2;

            if (nums[mid] < nums[mid + 1]){
                left = mid + 1;
            }else{
                right = mid;
            }
        }

        return left;
    }
}

// OC: O(logn)
// SC: O(n)
```



