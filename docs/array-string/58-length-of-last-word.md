---
tags:
    - String
    - Top Interviews
    - Microsoft
---

# [58. Length of Last Word](https://leetcode.com/problems/length-of-last-word/)

Given a string `s` consisting of words and spaces, return *the length of the **last** word in the string.*

A **word** is a maximal substring consisting of non-space characters only.

 

**Example 1:**

```
Input: s = "Hello World"
Output: 5
Explanation: The last word is "World" with length 5.
```

**Example 2:**

```
Input: s = "   fly me   to   the moon  "
Output: 4
Explanation: The last word is "moon" with length 4.
```

**Example 3:**

```
Input: s = "luffy is still joyboy"
Output: 6
Explanation: The last word is "joyboy" with length 6.
```



**Solution:**

```java
class Solution {
    public int lengthOfLastWord(String s) {
        StringBuilder sb = new StringBuilder(s);
        int count = 0;
        int flag = 0;
        for(int i = s.length()-1; i >= 0; i--){
            if (s.charAt(i) == ' ' && flag == 0){
                continue;
            }

            if (s.charAt(i) != ' '){
                count = count + 1;
                flag = 1;
            }

            if (s.charAt(i) == ' ' && flag == 1){
                break;
            }

        }

        return count;
    }
}
```



```java
class Solution {
    public int lengthOfLastWord(String s) {
        int n = s.length();
        boolean find = false;
        int result = 0;
        for (int i = n - 1; i >= 0; i--){
            if (s.charAt(i) == ' '){
                if (find == false){
                    continue;
                }else{
                    return result;
                }
            }
            find = true;
            result++;


        }
        return result;
    }
}

// TC: O(n)
// SC: O(1)
```

看示例 2，s = " fly me to the moon "，为了计算最后一个单词的长度，我们需要计算最后一个单词的首尾字母的下标：

单词最后一个字母的下标，也就是最后一个非空格字符的下标，记作 i。上例中 i 是字母 n 的下标。
单词第一个字母的下标，我们可以找到在 i 左边的最近空格的下标，记作 j，那么 j+1 就是这个单词的第一个字母的下标。如果左边没有空格，那么 j=−1。
单词长度为i−j

写法一：手写循环

```java
class Solution {
    public int lengthOfLastWord(String s) {
        int i = s.length() - 1;
        while (s.charAt(i) == ' ') {
            i--;
        }

        int j = i - 1;
        while (j >= 0 && s.charAt(j) != ' ') {
            j--;
        }

        return i - j;
    }
}

```

#### 复杂度分析

- 时间复杂度：O(*n*)，其中 *n* 是 *s* 的长度。
- 空间复杂度：O(1)。



写法二：库函数
先去掉末尾的空格，得到字符串 s ′ 。

设s′的长度为 m，那么 m−1 就是最后一个字母的下标。

找s′中最后一个空格的下标 j（如果不存在则 j=−1），那么答案为m−1−i

```java
class Solution {
    public int lengthOfLastWord(String s) {
        s = s.stripTrailing();
        return s.length() - 1 - s.lastIndexOf(' ');
    }
}

```



```java
class Solution {
    public int lengthOfLastWord(String s) {
        int n = s.length();
        int result = 0;
        for (int i = n - 1; i >= 0; i--){
            char cur = s.charAt(i);
            if (cur != ' '){
                while(i >= 0 && s.charAt(i) != ' '){
                    result++;
                    i--;
                }
                break;
            }
        }   

        return result;
    }
}

// TC: O(n)
// SC: O(1)
```

