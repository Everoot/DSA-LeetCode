# [10. Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/)

Given an input string `s` and a pattern `p`, implement regular expression matching with support for `'.'` and `'*'` where:

- `'.'` Matches any single character.
- `'*'` Matches zero or more of the preceding element.

The matching should cover the **entire** input string (not partial).

 

**Example 1:**

```
Input: s = "aa", p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```

**Example 2:**

```
Input: s = "aa", p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

**Example 3:**

```
Input: s = "ab", p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

 

**Constraints:**

- `1 <= s.length <= 20`
- `1 <= p.length <= 20`
- `s` contains only lowercase English letters.
- `p` contains only lowercase English letters, `'.'`, and `'*'`.
- It is guaranteed for each appearance of the character `'*'`, there will be a previous valid character to match.



**Solution:**

这个问题是要判断输入字符串`s` 是否与模式 `p` 匹配



```java
class Solution {
    boolean[][] dp;
    public boolean isMatch(String s, String p) {
        dp = new boolean[s.length() + 1][p.length() + 1];
        return helper(0, 0, s, p);
    }
    
    public boolean helper(int i, int j, String s, String p) {
        if (dp[i][j]) {
            return true;
        }
        if (i >= s.length() && j >= p.length()) {
            return true;
        }
        if (j >= p.length()) {
            return false;
        }
        // 检查当前字符是否匹配
        boolean match = (i < s.length() && (s.charAt(i) == p.charAt(j) || p.charAt(j) == '.'));
        // 处理*
        if (j + 1 < p.length() && p.charAt(j + 1) == '*') {
            dp[i][j] = (match && helper(i + 1, j, s, p)) || helper(i, j + 2, s, p);
            return dp[i][j];
        }
      // 如果p的下一个字符是*, 需要考虑两种情况:
      // match && helper(i+1, s,p) 当前字符匹配, 且我们向前移动s的一个字符, 同时
      // 保持j不变 即使*来匹配s中的一个字符
      // helper(i, j+2, s,p) 跳过当前字符*即把*当作匹配字符
        
        if (match) {
            dp[i][j] = helper(i + 1, j + 1, s, p);
            return dp[i][j];
          // 如果match 为true 
          // 检查s 和 p的下一个字符 分别是i+1, j+1
        }
        
        return false;
    }
}
```



```java
class Solution {
    public boolean isMatch(String s, String p) {
        boolean[][] dp = new boolean[s.length() + 1][p.length() + 1];
        dp[s.length()][p.length()] = true;
        
        for (int i = s.length(); i >= 0; i--){
            for (int j = p.length() - 1; j >= 0; j--){
                boolean first_match = i < s.length() && 
                (p.charAt(j) == s.charAt(i) || p.charAt(j) == '.');
                if (j + 1 < p.length() && p.charAt(j + 1) == '*'){
                    dp[i][j] = dp[i][j + 2] || first_match && dp[i + 1][j];
                }else{
                    dp[i][j] = first_match && dp[i + 1][j + 1];
                }
            }
        }
        return dp[0][0];
    }
}
```

