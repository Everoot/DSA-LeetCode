---
tags:
    - Dynamic Programming
    - Bit Manipulation
    - Top Interviews
---

# [338. Counting Bits](https://leetcode.com/problems/counting-bits/)

Given an integer `n`, return *an array* `ans` *of length* `n + 1` *such that for each* `i` (`0 <= i <= n`)*,* `ans[i]` *is the **number of*** `1`***'s** in the binary representation of* `i`.

 

**Example 1:**

```
Input: n = 2
Output: [0,1,1]
Explanation:
0 --> 0
1 --> 1
2 --> 10
```

**Example 2:**

```
Input: n = 5
Output: [0,1,1,2,1,2]
Explanation:
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101
```

 

**Constraints:**

- `0 <= n <= 105`



题看不懂系列: 那就硬看 

**Solution:**

```java
class Solution {
    public int[] countBits(int n) {
        int[] dp = new int[n+1];
        int offset = 1;

        for (int i = 1; i < n + 1; i++){
            if (offset * 2 == i){
                offset = i;
            }
            dp[i] = dp[i - offset] + 1;
        }

        return dp;
    }
}

// TC: O(n)
// SC: O(n) / O(1)

// n = 2
// n
// n+1
//  0 1 2         -> 0: 0 ,     1:1 ,   2:10
// [ 0, 1, 1]
// 
// [0, 1, 1]

// 0,    0
// 1,    1
// 2,   10
// 3,   11
// 4   100
// 5   101
// 6   110
// 7   111
// 8  1000


// 
```

