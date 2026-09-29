---
tags:
    - Array
    - Two Pointers
    - Top Interviews
---



# [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each unique element appears only **once**. The **relative order** of the elements should be kept the **same**. Then return *the number of unique elements in* `nums`.

Consider the number of unique elements of `nums` to be `k`, to get accepted, you need to do the following things:

- Change the array `nums` such that the first `k` elements of `nums` contain the unique elements in the order they were present in `nums` initially. The remaining elements of `nums` are not important as well as the size of `nums`.
- Return `k`.

**Custom Judge:**

The judge will test your solution with the following code:

```
int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

If all assertions pass, then your solution will be **accepted**.

 

**Example 1:**

```
Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**Example 2:**

```
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```



**Solution:**

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int i = 1;
        for (int j = 1; j < nums.length; j++){
            if (nums[j] != nums[j-1]){
                nums[i] = nums[j];
                i++;

            }
        }

        return i;
    }
}

// TC: O(n)
// SC: O(1)
```



```java
class Solution {
    public int removeDuplicates(int[] nums){
        if (nums == null){
            return 0;
        }
        
        if (nums.length <= 1){
            return nums.length;
        }

        int slow = 0;
        for (int fast = 1; fast < nums.length; fast++){
            if (nums[fast] == nums[slow]){
                continue;
            }else{
                slow++;
                nums[slow] = nums[fast];
            }
        }

        return slow + 1;
    }
}

/*
 [1,1,2]
    s
      f
// TC: O(n)
// SC: O(1)

*/
```



```java
class Solution {
    public int removeDuplicates(int[] nums) {
        // base case 

        if (nums == null || nums.length == 0){
            return 0;
        }

        if (nums.length == 1){
            return 1;
        }

        int stackSize = 0;

        for (int i = 1; i < nums.length; i++){
            if (nums[stackSize] != nums[i]){
                stackSize++;
                nums[stackSize] = nums[i];
            }
        }

        return stackSize + 1;
    }
}
```



```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int stackSize = 1;

        for (int i = 1; i < nums.length; i++){
            if (nums[i] != nums[i - 1]){
                nums[stackSize] = nums[i];
                stackSize++;
            }
        }

        return stackSize;
    }
}
```



```java
class Solution {
    public int removeDuplicates(int[] nums) {
       int n = nums.length;

       int result = 0;

       int left = 1;

       for (int right = 1; right < n; right++){
        if (nums[right] == nums[left - 1]){
            continue;
        }

        swap(nums, left, right);
        left++;
       }

       return left;
    }

    private void swap(int[] nums, int i, int j){
        int cur = nums[i];
        nums[i] = nums[j];
        nums[j] = cur;
    }
}
```

