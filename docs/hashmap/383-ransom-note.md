---
tags:
    - Hash Table
    - String
    - Counting
    - Top Interviews
---

# [383. Ransom Note](https://leetcode.com/problems/ransom-note/)

Given two strings `ransomNote` and `magazine`, return `true` *if* `ransomNote` *can be constructed by using the letters from* `magazine` *and* `false` *otherwise*.

Each letter in `magazine` can only be used once in `ransomNote`.

 

**Example 1:**

```
Input: ransomNote = "a", magazine = "b"
Output: false
```

**Example 2:**

```
Input: ransomNote = "aa", magazine = "ab"
Output: false
```

**Example 3:**

```
Input: ransomNote = "aa", magazine = "aab"
Output: true
```

 

**Constraints:**

- `1 <= ransomNote.length, magazine.length <= 105`
- `ransomNote` and `magazine` consist of lowercase English letters.



**Solution:**

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
       Map<Character, Integer> map = new HashMap<>();

       for (int i = 0; i < magazine.length(); i++){
        char cur = magazine.charAt(i);
        map.put(cur, map.getOrDefault(cur, 0) + 1);
       } 

       for (int i = 0; i < ransomNote.length(); i++){
        char cur = ransomNote.charAt(i);
        if (!map.containsKey(cur)){
            return false;
        }else{
            int count = map.get(cur);
            if (count < 1){
                return false;
            }else{
                map.put(cur, count - 1);
            }
        }
       }
       return true;
    }
}


// TC: O(n)
// SC: O(n)
```

