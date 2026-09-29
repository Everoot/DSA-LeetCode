---
tags:
    - Array
    - Binary Search 
    - Top Interveiws
---

# [153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

Suppose an array of length `n` sorted in ascending order is **rotated** between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:

- `[4,5,6,7,0,1,2]` if it was rotated `4` times.
- `[0,1,2,4,5,6,7]` if it was rotated `7` times.

Notice that **rotating** an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` of **unique** elements, return *the minimum element of this array*.

You must write an algorithm that runs in `O(log n) time.`

 

**Example 1:**

```
Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
```

**Example 3:**

```
Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times. 
```

 

**Constraints:**

- `n == nums.length`
- `1 <= n <= 5000`
- `-5000 <= nums[i] <= 5000`
- All the integers of `nums` are **unique**.
- `nums` is sorted and rotated between `1` and `n` times.



**Solution**

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0;
        int right = nums.length - 1 -1;

        while(left <= right){
            int mid = left + (right - left)/2;
            if (nums[mid] > nums[nums.length - 1]){
                left = mid + 1;
            }else{
                right = mid -1;
            }
        }

        return nums[left];
    }
}
```



```java
class Solution {
    public int findMin(int[] nums) {
        int n = nums.length;
        int left = 0;
        int right = n - 1;

        /*
         0 1 2 3 4 5 6
        [4,5,6,7,0,1,2]
               l
                 r
                 m          

        */

        // [0, r]
        // 

        while(left < right){
            int mid = left + (right - left)/2; // 0 + 6 - 0 /2 =3    // 3 + 6 -3 /2 = 3 + 3/2 = 4
            if (nums[mid] < nums[n-1]){
                right = mid;
            }else{
                left = mid + 1;
            }
        }

        return nums[left];
    }
}

// TC: O(logn)
// SC: O(n)
```



```java
class Solution {
    public int findMin(int[] nums) {
        int res = nums[0];

        int left = 0;
        int right = nums.length -1;

        while (left <= right){
            if (nums[left] < nums[right]){
                res = Math.min(res, nums[left]);
                break;
            }

            int mid = left + (right - left)/ 2;
            res = Math.min(nums[mid], res);

            if (nums[mid] >= nums[left]){
                left = mid + 1;
            }else{
                right = mid -1;
            }

        }

        return res;
    }
}

// TC: O(logn)
// SC: O(1)
```



```java
class Solution {
    public int findMin(int[] nums) {
        if (nums.length == 1){
            return nums[0];
        }

        int left = 0;
        int right = nums.length -1;

        if (nums[0] < nums[right]){
            return nums[0];
        } 


        while(left <= right){
            int mid = left + (right - left)/2;

            if (nums[mid] > nums[mid+1]){
                return nums[mid+1];
            }

            if (nums[mid] < nums[mid-1]){
                return nums[mid];
            }

            if (nums[mid] > nums[0]){
                left = mid + 1;
            }else{
                right = mid -1;
            }
        }


        return Integer.MAX_VALUE;
    }
}
// TC: O(logn)
// SC: O(1)
```

题看不懂系列, 不知道它要我干啥

原本是个sorted的array, 然后最小的和最尾巴穿起来, 像是佛珠一样, 第一个到最后一个,这么移动, 然后要找到最小的那个 使用O(logn)的时间 -> 可这种搜索时间的限制, 很自然地想到BinarySearch.

```java
class Solution {
    public int findMin(int[] nums) {
        Arrays.sort(nums);
        return nums[0];
    }
}
```

