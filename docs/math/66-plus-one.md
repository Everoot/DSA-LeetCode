---
tags:
    - Array
    - Math
    - Top Interviews
---

# [66. Plus One](https://leetcode.com/problems/plus-one/)

You are given a **large integer** represented as an integer array `digits`, where each `digits[i]` is the `ith` digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading `0`'s.

Increment the large integer by one and return *the resulting array of digits*.

 

**Example 1:**

```
Input: digits = [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
Incrementing by one gives 123 + 1 = 124.
Thus, the result should be [1,2,4].
```

**Example 2:**

```
Input: digits = [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.
Incrementing by one gives 4321 + 1 = 4322.
Thus, the result should be [4,3,2,2].
```

**Example 3:**

```
Input: digits = [9]
Output: [1,0]
Explanation: The array represents the integer 9.
Incrementing by one gives 9 + 1 = 10.
Thus, the result should be [1,0].
```

 

**Solution:**

题目让我们模拟加一运算。

比如 23999+1=24000，999+1=1000。

算法：

1. 从右往左找第一个不等于 9 的数，记作 digits[i]。
2. 进位，把 digits[i] 加一，把下标在 [i+1,n−1] 中的数全变成 0。
3. 特别地，如果所有数都等于 9，那么答案为 [1,0,0,…,0]，其中有 n 个 0。



```java
class Solution {
    public int[] plusOne(int[] digits) {
        int n = digits.length;
        for (int i = n - 1; i >= 0; i--){
            if (digits[i] < 9){
                digits[i]++; // 进位
                return digits;
            }
            digits[i] = 0; // 进位数字的右边数字变成 0
        }

        // digits 全是9, 加一后变成100...00
        int[] ans = new int[n + 1];

        ans[0] = 1;
        return ans;
    }
}

// 时间复杂度：O(m)，其中 m 是 digits 末尾 9 的个数。
// 空间复杂度：O(1)。返回值不计入。
```



```java
class Solution {
    public int[] plusOne(int[] digits) {
        // base case 
        if (digits == null || digits.length == 0){
            return new int[]{1};
        }

        if (digits.length == 1 && digits[0] <= 8){
            digits[0] = digits[0] + 1;
            return digits;
        }

        if (digits.length == 1 && digits[0] == 9){
            return new int[]{1,0};
        }

        digits[digits.length - 1] = digits[digits.length -1] + 1;

        int[] result = new int[digits.length + 1];
        int carry = 0;
        for (int i = digits.length - 1; i >= 0; i--){
            digits[i] = digits[i] + carry;
            carry = digits[i] / 10;
            digits[i] = digits[i] % 10;
            result[i+1] = digits[i];
        }

        result[0] = carry;

        if (result[0] != 0){
            return result;
        }else{
            return digits;
        }
        
    }
}

// TC: O(n)
// SC: O(n)
```



