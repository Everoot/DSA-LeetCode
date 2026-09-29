---
tags:
    - Array
    - Binary Search
    - Top Interviews
    - Microsoft
---

# [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return *the index of* `target` *if it is in* `nums`*, or* `-1` *if it is not in* `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

 

**Example 1:**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

**Example 3:**

```
Input: nums = [1], target = 0
Output: -1
```

 

**Constraints:**

- `1 <= nums.length <= 5000`
- `-104 <= nums[i] <= 104`
- All values of `nums` are **unique**.
- `nums` is an ascending array that is possibly rotated.
- `-104 <= target <= 104`



**Solution**

```java
class Solution {
    public int search(int[] nums, int target) {
        int min = findMin(nums);

        if (nums[min] == target){
            return min;
        }

        if (target <= nums[nums.length - 1]){
            return binarySearch(nums, target, min, nums.length - 1);
        }else{
            return binarySearch(nums, target, 0, min - 1);
        }

    }


    private int findMin(int[] nums){
        int left = 0;
        int right = nums.length - 1 - 1;
        while(left <= right){
            int mid = left + (right - left) / 2;
            if (nums[mid] < nums[nums.length - 1]){
                right = mid - 1;
            }else{
                left = mid + 1;
            }
        }

        return left;
    }

    private int binarySearch(int[] nums, int target, int left, int right){
        while(left <= right){
            int mid = left + (right - left)/2;
            if (nums[mid] == target){
                return mid;
            }else if (nums[mid] < target){
                left = mid + 1;
            }else{
                right = mid - 1;
            }
        }

        if (nums[left] != target){
            return -1;
        }else{
            return left;
        }
    }
}
```



```java
class Solution {
    public int search(int[] nums, int target) {
        int min = findMin(nums);

        if (target > nums[nums.length - 1]){ // target 在第一段
            return lowerBound(nums, 0, min -1, target);
        }else{
            // target 在第二段
            return lowerBound(nums, min, nums.length - 1, target);
        }

    }


    // 153 
    private int findMin(int[] nums){
        int left = 0;
        int right = nums.length - 1 - 1;
        while(left <= right){
            int mid = left + (right - left)/2;
            if (nums[mid] < nums[nums.length - 1]){
                right = mid - 1;
            }else{
                left = mid + 1;
            }
        }

        return left;
    } 

    // 有序数组中找target的下标
    private int lowerBound(int[] nums, int left, int right, int target){
        while(left <= right){
            int mid = left + (right - left)/2;
            if (nums[mid] < target){
                left = mid + 1;
            }else{
                right = mid - 1;
            }
        }

        return nums[left] == target ? left : -1;
    }
}
```



```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;

        while (left <= right){ // Case 1: find target
            int mid = left + (right - left)/2;

            if (nums[mid] == target){
                return mid;
            }else if (nums[mid] >= nums[left]){ // Case 2: subarray on mid's left is sorted    // must have =
                if (target >= nums[left] && target < nums[mid]){
                    right = mid-1;
                }else{
                    left = mid +1;
                }
            }else{ // Case 3: subarray on mid's right is sorted
                if (target <= nums[right] && target > nums[mid]){
                    left = mid + 1;
                }else{
                    right = mid -1;
                }
            }
        }
        return -1;
    }
}

// TC: O(logn)
// SC: O(1)
```

Binary search的题边界要注意



```java
class Solution {
    public int search(int[] nums, int target) {
        int n = nums.length;

        int left = 0;
        int right = n - 1;

        /*
            [4,5,6,7,0,1,2]
                     l
                         r
                       m
        */

        while(left <= right){
            int mid = left + (right - left)/2;

            if (nums[mid] == target){
                return mid;
            }else if (nums[left] <= nums[mid]){
                if (nums[left] <= target && target <= nums[mid]){
                    right = mid - 1;
                }else{
                    left = mid + 1;
                }
            }else{
                // nums[left] > nums[mid]
                if (nums[mid] <= target && target <= nums[right]){
                    left = mid +1;
                }else{
                    right = mid -1;
                }
            }
        }
        return -1;
    }
}

// TC: O(logn)
// SC: O(n)
```



```java
class Solution {
    public int search(int[] nums, int target) {
        int n = nums.length;
        int left = 0;
        int right = n-1;

        if (n == 1){
            if (nums[0] != target){
                return -1;
            }else{
                return 0;
            }
        }

        /*  0 1 2 3 4 5 6
            4 5 6 7 0 1 2
                                t = 3
            l 
                         r
                  m           l + (r - l ) /2  = 0 + (6) /2 = 3

        if (nums[left] < nums[right]) -> in order sorted
        
        /// not order
        if (target < nums[right]) -> right
        if (target > nums[left]) -> left part

        */
        /*
             0  1
            [3, 1]
             l
                 r
                      0 + (1- 0) /2 = 0
             m

              0 1 2 3 4 5 6 7
             [4,5,6,7,8,1,2,3]

                      l     
                            r
                        m              0 + (7 - 0) /2 3      
        */

        
        while(left <= right){
            int mid = left + (right - left)/2;

            if (nums[mid] == target){
                return mid;
            }else{
                // move
                if (nums[left] < nums[right]){
                    if (nums[mid] < target){
                        left = mid + 1;
                    }else{
                        right = mid - 1;
                    }
                }else{
                    // nums[left] > nums[right]
                    if (target == nums[right]){
                        return right;
                    }

                    if (target == nums[left]){
                        return left;
                    }
                    if (target < nums[right]){
                        left = left + 1;
                    }else{
                        right = right - 1;
                    }
                }
            }
        }

        return -1;
    }
}


// TC: O(logn)
// SC: O(1)
```

