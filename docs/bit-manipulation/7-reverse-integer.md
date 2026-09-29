---
tags:
    - Math
    - Microsoft
---



# [7. Reverse Integer](https://leetcode.com/problems/reverse-integer/)

Given a signed 32-bit integer `x`, return `x` _with its digits reversed_. If reversing `x` causes the value to go outside the signed 32-bit integer range `[-231, 231 - 1]`, then return `0`.

**Assume the environment does not allow you to store 64-bit integers (signed or unsigned).**

**Example 1:**

```
Input: x = 123
Output: 321
```

**Example 2:**

```
Input: x = -123
Output: -321
```

**Example 3:**

```
Input: x = 120
Output: 21
```

**Constraints:**

- `-231 <= x <= 231 - 1`

Solution:

```java
class Solution {
    public int reverse(int x) { // 123
        int result = 0;
        // Integer.MAX_VALUE 2147483647
        // −2147483648
        while(x != 0){ // 123
            int pop = x % 10; // get last number 3
            x = x / 10; // remove last number  123 /10 =12 
            // check if reversing x will cause overflow
            // because  result = result * 10 + pop; 
            if (result > Integer.MAX_VALUE /10 || result == Integer.MAX_VALUE/ 10 && pop > 7){
                return 0;
            }

            if (result < Integer.MIN_VALUE / 10 || result == Integer.MIN_VALUE/10 && pop < -8){
                return 0;
            }

            result = result * 10 + pop; // 
        }

        return result;
        
    }
}

// TC: O(n)
// SC: O(1)
```
