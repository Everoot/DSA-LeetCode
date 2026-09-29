---
tags:
    - Array
    - Bit Manipulation
    - Top Interviews
---

# [136. Single Number](https://leetcode.com/problems/single-number/)

Given a **non-empty** array of integers `nums`, every element appears *twice* except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

 

**Example 1:**

```
Input: nums = [2,2,1]
Output: 1
```

**Example 2:**

```
Input: nums = [4,1,2,1,2]
Output: 4
```

**Example 3:**

```
Input: nums = [1]
Output: 1
```

 

**Constraints:**

- `1 <= nums.length <= 3 * 104`
- `-3 * 104 <= nums[i] <= 3 * 104`
- Each element in the array appears twice except for one element which appears only once.



**Solution:**

Using the property of the XOR operation $a \oplus a = 0$, we can use XOR to "cancel out" all elements that appear twice -- leaving only the elemnt that appears once.

For example, give $nums = [4, 1, 2, 1, 2]$, if we XOR all the elements:
$$
4 \oplus 1 \oplus 2 \oplus 1 \oplus2 = 4 \oplus (1 \oplus 1) \oplus (2 \oplus 2)=4 \oplus 0 \oplus 0=4
$$
Here we used two important XOR properties:

* Commutative law: $a \oplus b = b \oplus a $
* Associative law: $(a \oplus b) \oplus c = a \oplus (b \oplus c)$

In the code, intializing $ans = 0$ works because $0 \oplus a = a$, which is equivalent to starting from the first number and XORing all other numbers one by one.

```java
class Solution {
    public int singleNumber(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        for (int i = 0; i < nums.length; i++){
            if (!set.contains(nums[i])){
                set.add(nums[i]);
            }else{
                set.remove(nums[i]);
            }

        }

        for (int i : set){
            return i;

        }

        return -1;
        
    }
}

// TC: O(n)
// SC: O(n)
```



```java
class Solution {
    public int singleNumber(int[] nums) {
        int result = 0;
        for (int i = 0; i < nums.length; i++){
            result = nums[i] ^ result;
        }
        return result;
    }
}
// TC: O(n)
// SC: O(1)

// 0000000
//  1 ^ 1  xor = 0
```

