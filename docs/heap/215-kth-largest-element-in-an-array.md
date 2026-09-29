---
tags:
    - Array
    - Divide and Conquer
    - Sorting
    - Heap (Priority Queue)
    - Quickselect
---

# [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

Given an integer array `nums` and an integer `k`, return *the* `kth` *largest element in the array*.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

Can you solve it without sorting?

**Example 1:**

```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

**Example 2:**

```
Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4
```

**Solution:**

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<Integer>((a, b) -> (b - a));

        for (int i = 0; i < nums.length; i++){
            pq.offer(nums[i]);
        }

        for (int i = 0; i < k - 1; i++){
            pq.poll();
        }

        return pq.poll();
    }
}

// TC: O(nlogn)
// SC: O(n)
```



```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        Arrays.sort(nums);
        return nums[nums.length - k];
    }
}
// TC: O(nlogn)
// SC: O(logn)
```



```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<Integer>();
        
        for (int i = 0; i < nums.length; i++){
            if (pq.size() < k){
                pq.offer(nums[i]);
            }else{
                if (pq.peek() < nums[i]){
                    pq.poll();
                    pq.offer(nums[i]);
                }
            }
        }

        return pq.peek();

    }
}

// TC: O(n*logn)
// SC: O(k)
```



