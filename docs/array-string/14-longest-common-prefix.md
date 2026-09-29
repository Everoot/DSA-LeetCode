---
tags:
    - Array
    - String
    - Trie
    - Top Interview 150
---

# [14. Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

 

**Example 1:**

```
Input: strs = ["flower","flow","flight"]
Output: "fl"
```

**Example 2:**

```
Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

 

**Constraints:**

- `1 <= strs.length <= 200`
- `0 <= strs[i].length <= 200`
- `strs[i]` consists of only lowercase English letters if it is non-empty.



**Solution:**

看示例 1：

flower
flow
flight


从左到右，竖着看，第一列全是 f，第二列全是 l，第三列就不全一样了，所以「最长公共前缀」是 fl。

具体算法如下：

1. 从左到右遍历 strs 的每一列。
2. 设当前遍历到第 j 列，从上到下遍历这一列的字母。
3. 设当前遍历到第 i 行，即 $strs[i][j]$。如果 j 等于 $strs[i]$ 的长度，或者 $strs[i][j] \ne strs[0][j]$，说明这一列的字母缺失或者不全一样，那么最长公共前缀的长度等于 j，返回 strs[0] 的长为 j 的前缀。
4. 如果没有中途返回，说明所有字符串都有一个等于 strs[0] 的前缀，那么最长公共前缀就是 strs[0]。

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        String s0 = strs[0];
        for (int j = 0; j < s0.length(); j++){ //从左到右
            char c = s0.charAt(j);
            for (String s : strs){ // 从上到下
                if (j == s.length() || s.charAt(j) != c){ // 这一列有字母缺失或者不同
                    return s0.substring(0, j); // 0 到j-1是公共前缀
                }
            }
        }

        return  s0;
    }
}

// 时间复杂度：O(nm)，其中 n 为 strs 的长度，m 为 strs 中最短字符串的长度。
// 空间复杂度：O(1)。返回值不计入。
```



```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        int n = strs.length;
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 200; i++){
           String curString = strs[0]; // ""
           if (curString.length() <= i){ // 0 < 0
            return sb.toString(); 
           }
           char curChar = curString.charAt(i);
           for (int j = 0; j < n; j++){
            String nextString = strs[j];
            if (nextString.length() <= i){
                return sb.toString();
            }

            char nextChar = nextString.charAt(i);
            if (nextChar != curChar){
                return sb.toString();
            }
           }
           sb.append(curChar); 
        }

        return sb.toString();   
    }

}

// TC: O(n * m)
// SC: O(1)
```

