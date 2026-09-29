---
tags:
    - Divide and Conquer
    - Bit Manipulation
    - Top Interviews
---



# [191. Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

Write a function that takes the binary representation of a positive integer and returns the number of set bits it has (also known as the [Hamming weight](http://en.wikipedia.org/wiki/Hamming_weight)).

**Example 1:**

**Input:** n = 11

**Output:** 3

**Explanation:**

The input binary string **1011** has a total of three set bits.

**Example 2:**

**Input:** n = 128

**Output:** 1

**Explanation:**

The input binary string **10000000** has a total of one set bit.

**Example 3:**

**Input:** n = 2147483645

**Output:** 30

**Explanation:**

The input binary string **1111111111111111111111111111101** has a total of thirty set bits.

**Constraints:**

- `1 <= n <= 231 - 1` 

**Follow up:** If this function is called many times, how would you optimize it?

**Solution:**

```java
class Solution {
    public int hammingWeight(int n) {
        int result = 0;
        while (n != 0){
            if (n % 2 != 0){
                result++;
            }
            n = n >> 1;
        }
        return result;
    }
}
// TC: O(logn)
// SC: O(1)
```

