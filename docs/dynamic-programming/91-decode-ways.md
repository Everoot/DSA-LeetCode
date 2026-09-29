---
tags:
    - String
    - Dynamic Programming
    - Remember
    - Top Interviews
---

# [91. Decode Ways](https://leetcode.com/problems/decode-ways/)

A message containing letters from `A-Z` can be **encoded** into numbers using the following mapping:

```
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```

To **decode** an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, `"11106"` can be mapped into:

- `"AAJF"` with the grouping `(1 1 10 6)`
- `"KJF"` with the grouping `(11 10 6)`

Note that the grouping `(1 11 06)` is invalid because `"06"` cannot be mapped into `'F'` since `"6"` is different from `"06"`.

Given a string `s` containing only digits, return *the **number** of ways to **decode** it*.

The test cases are generated so that the answer fits in a **32-bit** integer.

 

**Example 1:**

```
Input: s = "12"
Output: 2
Explanation: "12" could be decoded as "AB" (1 2) or "L" (12).
```

**Example 2:**

```
Input: s = "226"
Output: 3
Explanation: "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

**Example 3:**

```
Input: s = "06"
Output: 0
Explanation: "06" cannot be mapped to "F" because of the leading zero ("6" is different from "06").
```

 

**Constraints:**

- `1 <= s.length <= 100`
- `s` contains only digits and may contain leading zero(s).



**Solution:**



### DFS

```java
class Solution {
    public int numDecodings(String s) {
       int n = s.length();
       return dfs(n - 1, s); 
    }

    private int dfs(int i, String s){
        if (i < 0){
            return 1;
        }

        int result = 0;

        char cur = s.charAt(i);
        
        if (cur != '0'){
            result = result + dfs(i - 1, s);
        }

        if (i - 1 >= 0){
            int num = (s.charAt(i - 1) - '0') * 10 + (cur - '0');
            if (num >= 10 && num <= 26){
                result = result + dfs(i - 2, s);
            }
        }

        return result;
    }
}

// TLE

// TC: O(2^n)
// SC: O(n)
```





### DFS + memo

```java
class Solution {
    public int numDecodings(String s) {
       int n = s.length();
       int[] memo = new int[n];
       Arrays.fill(memo, -1);
       return dfs(n - 1, s, memo); 
    }

    private int dfs(int i, String s, int[] memo){
        if (i < 0){
            return 1;
        }

        if (memo[i] != -1){
            return memo[i];
        }

        int result = 0;

        char cur = s.charAt(i);
        
        if (cur != '0'){
            result = result + dfs(i - 1, s, memo);
        }

        if (i - 1 >= 0){
            int num = (s.charAt(i - 1) - '0') * 10 + (cur - '0');
            if (num >= 10 && num <= 26){
                result = result + dfs(i - 2, s, memo);
            }
        }

        return memo[i] = result;
    }
}

// TC: O(n)
// SC: O(n)
```





```java
class Solution {
    public int numDecodings(String s) {
        // DP array to store the subproblem results
        int[] dp = new int[s.length() + 1];  // 226     dp[_, _ , _, _]
        dp[0] = 1; // 表示空字符串的解码方式数量，设为1，因为一个空字符串可以有一种解码方式，即什么也不做
        //  dp[i] 表示字符串 s 的前 i 个字符可以被解码的不同方式的总数。
        // 这里的 i 是从 1 开始计数的，对应到字符串 s 的索引是从 0 到 i-1
        
        // Ways to decode a string of size 1 is 1. Unless the string is '0'.
        // '0' doesn't have a single digit decode.
        if (s.charAt(0) == '0'){
            dp[1] = 0;
        }else{
            dp[1] = 1;
        }
        // 012
        // 226
        //  | 
        // dp[1, 1, 2,2]
        // 

        for(int i = 2; i < dp.length; i++) {
            // i  = 3 
            // Check if successful single digit decode is possible.
            if (s.charAt(i - 1) != '0') {// 2-1 =1  // 6
               dp[i] = dp[i - 1];   // dp[2] = dp[i-1] = dp[2-1] = dp[1] = 1
              // dp[4] = dp[i-1] =2 
            }
            
            // Check if successful two digit decode is possible.
            int twoDigit = Integer.valueOf(s.substring(i - 2, i));
            //s.substring() : [i-2,i] = (2 -2 ,i) = 0, 2    22
            // 3 -2 , 3. 1, 3, 1,2    26
            if (twoDigit >= 10 && twoDigit <= 26) { // 
                dp[i] = dp[i] + dp[i - 2]; // dp[2] = dp[2] + dp[i-2] = dp[2-2] = dp[2] + dp[0]
                // 1 + 1 = 2
            }
        }
        
        return dp[s.length()];
    }
}

// TC: O(n)
// SC: O(n)
```

