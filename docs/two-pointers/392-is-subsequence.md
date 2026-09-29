---
tags:
    - Two Pointers
    - Top Interview 150
---

# [392. Is Subsequence](https://leetcode.com/problems/is-subsequence/)

Given two strings `s` and `t`, return `true` *if* `s` *is a **subsequence** of* `t`*, or* `false` *otherwise*.

A **subsequence** of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

 

**Example 1:**

```
Input: s = "abc", t = "ahbgdc"
Output: true
```

**Example 2:**

```
Input: s = "axc", t = "ahbgdc"
Output: false
```



**Solution:**

不要想的太复杂

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        if (s == null || s.length() == 0){
            return true;
        }

        int i = 0;
        int j = 0;

        while(i < s.length() && j < t.length()){
            if (s.charAt(i) == t.charAt(j)){
                i++;
                j++;
            }else{
                j++;
            }
        }

        if (i == s.length()){
            return true;
        }else{
            return false;
        }

    }
}

// TC: O(n)
// SC: O(1)
```



```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        // base case 
        if (s == null && t == null){
            return true;
        }

        if (s == null || t == null){
            return false;
        }

        if (s.length() > t.length()){
            return false;
        }

        int tpointer = 0;
        int spointer = 0;

        while (tpointer < t.length()){
            if (spointer == s.length()){
                return true;
            }
            if (t.charAt(tpointer) == s.charAt(spointer)){
                tpointer++;
                spointer++;
            }else{
                tpointer++;
            }
        }

        if (spointer == s.length()){
            return true;
        }else{
            return false;
        }
    }
}

// TC: O(n)
// SC: O(1)
/*


while( t< length)

       a h b g d c
                   t

       a b c
             s


s == s.length() 
true

*/
```

