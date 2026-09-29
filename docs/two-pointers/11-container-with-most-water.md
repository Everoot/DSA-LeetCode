---
tags:
    - Array
    - Two Pointers
    - Greedy
    - Top Interviews
---

# [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return *the maximum amount of water a container can store*.

**Notice** that you may not slant the container.

 

**Example 1:**

![img](./11. Container With Most Water/question_11.jpg)

```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```

**Example 2:**

```
Input: height = [1,1]
Output: 1
```

 

**Constraints:**

- `n == height.length`
- `2 <= n <= 105`
- `0 <= height[i] <= 104`



**Solution:**

```java
class Solution {
    public int maxArea(int[] height) {
        int right = height.length - 1;
        int left = 0;

        int max = 0;
        while(left < right){
            int width = right - left;

            int h = Math.min(height[left], height[right]);

            max = Math.max(width * h, max);

            if (height[left] <= height[right]){
                left++;
            }else{
                right--;
            }

        }

        return max;

    }
}

// TC: O(n)
// SC: O(1)

/* 
 
 [1,8,6,2,5,4,8,3,7]
    l             r


*/
```



```java
class Solution {
    public int maxArea(int[] height) {
        int result = 0;

        /*
             0 1 2 3 4 5 6 7 8
            [1,8,6,2,5,4,8,3,7]
               i 
                - - - - - - - 
                             j

            lmax = 8 
            rmax = 7 
            area = Math.min(lmax, rmax) * (8-1)
                 =  7 *  7 = 49

        */

        int left = 0;
        int lmax = height[left];

        int right = height.length - 1;
        int rmax = height[right];

        while(left < right){
            int area = Math.min(lmax, rmax) * (right - left);
            result = Math.max(area, result);

            int curLeft = height[left];
            int curRight = height[right];

            if (curLeft <= curRight){
                left++;
                lmax = Math.max(height[left], lmax);
            }else{
                right--;
                rmax = Math.max(height[right], rmax);
            }


        }

        return result;
    }
}

// TC: O(n)

// SC: O(1)
```





