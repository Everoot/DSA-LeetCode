---
tags:
    - Hash Table
    - String
    - Top Interviews
---

# [205. Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/)

Given two strings `s` and `t`, *determine if they are isomorphic*.

Two strings `s` and `t` are isomorphic if the characters in `s` can be replaced to get `t`.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character, but a character may map to itself.

 

**Example 1:**

**Input:** s = "egg", t = "add"

**Output:** true

**Explanation:**

The strings `s` and `t` can be made identical by:

- Mapping `'e'` to `'a'`.
- Mapping `'g'` to `'d'`.

**Example 2:**

**Input:** s = "foo", t = "bar"

**Output:** false

**Explanation:**

The strings `s` and `t` can not be made identical as `'o'` needs to be mapped to both `'a'` and `'r'`.

**Example 3:**

**Input:** s = "paper", t = "title"

**Output:** true

 

**Constraints:**

- `1 <= s.length <= 5 * 104`
- `t.length == s.length`
- `s` and `t` consist of any valid ascii character.



Solution:

```java
class Solution {
    public boolean isIsomorphic(String s, String t) {
        if (s.length() != t.length()){
            return false;
        }

        if (s.length() == 1){
            return true;
        }

        char[] sArray = s.toCharArray();
        Map<Character, Character> map = new HashMap<>();

        for (int i = 0; i < s.length(); i++){
            if (map.containsKey(s.charAt(i)) || map.containsValue(t.charAt(i))){
                continue;
            }else{
                map.put(s.charAt(i), t.charAt(i));
            }
        }

        for (int i = 0; i < sArray.length; i++){
            char cur = sArray[i];
            if (!map.containsKey(cur)){
                return false;
            }else{
                char replaceCur = map.get(cur);
                sArray[i] = replaceCur;
            }
        }

        String replace = new String(sArray);

        if (replace.equals(t)){
            return true;
        }else{
            return false;
        }

    }
}

// TC: O(n)
// SC: O(n)
```



```java
class Solution {
    public boolean isIsomorphic(String s, String t) {
        int n = s.length();
        Map<Character, Character> map = new HashMap<>();
        Map<Character, Character> map2 = new HashMap<>();

        for (int i = 0; i < n; i++){
            char cur = s.charAt(i);
            char cur2 = t.charAt(i);
            map.put(cur, cur2); 
            map2.put(cur2, cur);
        }

        char[] sChar = s.toCharArray();
        char[] tChar = t.toCharArray();

        for (int i = 0; i < n; i++){
            char sCur = sChar[i];
            char replace = map.get(sCur);
            sChar[i] = replace;

            tChar[i] = map2.get(tChar[i]);
        }

        String replaceS = new String(sChar);
        String replaceT = new String(tChar);

        if (replaceS.equals(t) && replaceT.equals(s)){
            return true;
        }else{
            return false;
        }

    }
}

// TC: O(n)
// SC: O(n)
```

