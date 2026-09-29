---
tags:
    - Array
    - Dynamic Programming
    - Microsoft
---



# [152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

Given an integer array `nums`, find a subarray that has the largest product, and return *the product*.

The test cases are generated so that the answer will fit in a **32-bit** integer.

**Example 1:**

```
Input: nums = [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```

**Example 2:**

```
Input: nums = [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```



**Solution:**

```java
class Solution {
    public int maxProduct(int[] nums) {
        if (nums == null || nums.length == 0){
            return Integer.MIN_VALUE;
        }

        int max = nums[0];
        int min = nums[0];
        int result = nums[0];

        for (int i = 1; i < nums.length; i++){
            int cur = nums[i];

            int curMax = Math.max(max * cur, min * cur);
            int curMin = Math.min(max * cur, min * cur);

            max = Math.max(cur, curMax);
            min = Math.min(cur, curMin);

            result = Math.max(max, result);
        
        }

        return result;

    }
}
// TC: O(n)
// SC: O(1)

/*
			2,3,-2,4
cur		  3 -2 4
temp  - 6 -2 4
min   2 6 -12 -48
max   2 6 -2 4
res   2 6 6 6

*/
```



```java
class Solution {
    public int maxProduct(int[] nums) {
        if (nums == null || nums.length == 0){
            return 0;
        }

        int result = nums[0];

        for (int i = 0; i < nums.length; i++){
            int cur = 1;
            for (int j = i; j < nums.length; j++){
                cur = cur * nums[j];
                result = Math.max(result, cur);
            }
        }

        return result;
        
    }
}

// TC: O(n^2)
// SC: O(1)
```





