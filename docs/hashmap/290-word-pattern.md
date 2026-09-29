---
tags:
    - Hash Table
    - String
    - Top Interview 150
---

# [290. Word Pattern](https://leetcode.com/problems/word-pattern/)

Given a `pattern` and a string `s`, find if `s` follows the same pattern.

Here **follow** means a full match, such that there is a bijection between a letter in `pattern` and a **non-empty** word in `s`. Specifically:

- Each letter in `pattern` maps to **exactly** one unique word in `s`.
- Each unique word in `s` maps to **exactly** one letter in `pattern`.
- No two letters map to the same word, and no two words map to the same letter.

 

**Example 1:**

**Input:** pattern = "abba", s = "dog cat cat dog"

**Output:** true

**Explanation:**

The bijection can be established as:

- `'a'` maps to `"dog"`.
- `'b'` maps to `"cat"`.

**Example 2:**

**Input:** pattern = "abba", s = "dog cat cat fish"

**Output:** false

**Example 3:**

**Input:** pattern = "aaaa", s = "dog cat cat dog"

**Output:** false

 

**Constraints:**

- `1 <= pattern.length <= 300`
- `pattern` contains only lower-case English letters.
- `1 <= s.length <= 3000`
- `s` contains only lowercase English letters and spaces `' '`.
- `s` **does not contain** any leading or trailing spaces.
- All the words in `s` are separated by a **single space**.



**Solution:**

```java
class Solution {
    public boolean wordPattern(String pattern, String s) {
        Map<Character, String> mapPattern = new HashMap<>();
        Map<String, Character> mapS = new HashMap<>();
        int n = pattern.length();
        String[] sArray = s.split(" ");
        if (sArray.length != n){
            return false;
        }

        for (int i = 0; i < n; i++){
            char curPattern = pattern.charAt(i);
            String curS = sArray[i];

            if (!mapPattern.containsKey(curPattern) && !mapS.containsKey(curS)){
                mapPattern.put(curPattern, curS);
                mapS.put(curS, curPattern);
            }else if (mapPattern.containsKey(curPattern) && mapS.containsKey(curS)){
                String curMapPattern = mapPattern.get(curPattern);
                char curMapS = mapS.get(curS);
                if (!curMapPattern.equals(curS) || curMapS != curPattern){
                    return false;
                }
            }else{
                return false;
            }
        }
        return true;
    }
}

// TC: O(n)
// SC: O(n)
```

