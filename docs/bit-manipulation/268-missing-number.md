---
tags:
    - Array
    - Hash Table
    - Math
    - Binary Search
    - Bit Manipulation
    - Sorting
    - Top Interivews
---

#  [268. Missing Number](https://leetcode.com/problems/missing-number/)

Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return *the only number in the range that is missing from the array.*

 **Example 1:**

```
Input: nums = [3,0,1]
Output: 2
Explanation: n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums.
```

**Example 2:**

```
Input: nums = [0,1]
Output: 2
Explanation: n = 2 since there are 2 numbers, so all numbers are in the range [0,2]. 2 is the missing number in the range since it does not appear in nums.
```

**Example 3:**

```
Input: nums = [9,6,4,2,3,5,7,0,1]
Output: 8
Explanation: n = 9 since there are 9 numbers, so all numbers are in the range [0,9]. 8 is the missing number in the range since it does not appear in nums.
```



**Solution:**

Approach #1 Sorting:

```java
class Solution {
    public int missingNumber(int[] nums) {
        Arrays.sort(nums);
        int result = nums.length;

        for (int i = 0; i < nums.length; i++){
            if (nums[i] != i){
                result = i;
                break;
            }
        }

        return result;
    }
}

// TC: O(nlogn)
// SC: O(1)
```



Approach #2 HashSet:

```java
class Solution {
    public int missingNumber(int[] nums) {
        // base case 
        if (nums == null || nums.length == 0){
             return -1;
        }

        Set<Integer> set = new HashSet<Integer>();
        for (int i = 0; i < nums.length; i++){
            set.add(nums[i]);
        }

        int result = nums.length;

        for (int i = 0; i < nums.length; i++){
            if (!set.contains(i)){
                result = i;
                break;
            }
        }
        return result;
    }
}


// TC: O(n)
// SC: O(n)
```



Approach #3 Bit Manipulation:

```java
class Solution {
    public int missingNumber(int[] nums) {
        int missing = nums.length;
        for (int i = 0; i < nums.length; i++) {
            missing ^= i ^ nums[i];
        }
        return missing;
    }
}
```

$$
\begin{align}
result & = 3 \oplus (0 \oplus 3) \oplus (1 \oplus 0) \oplus (2 \oplus 1)\\
			& = b11 \oplus (b00 \oplus b11) \oplus (b01 \oplus b00) \oplus (b10 \oplus b01)\\	
			 & = b11 \oplus  b11 \oplus b01 \oplus b11 \\
		  	& = b00 \oplus b10 \\
				& = b10 \\
				& = 2
\end{align}
$$





Approach #4 Gauss' Formula:

```java
class Solution {
    public int missingNumber(int[] nums) {
        int expectedSum = (0 + nums.length) * (nums.length+1)  /2; 
        int sum = 0;
        for (int i = 0; i < nums.length; i++){
            sum = sum + nums[i];
        }
        int result = expectedSum - sum;
        return result;
    }
}

// TC: O(n)
// SC: O(1)

```

$$
\sum_{i=0}^n = \frac{n(n+1)}{2}
$$

