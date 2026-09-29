---
tags:
  - Hash Table
  - String
  - Sorting
  - Top Interviews
  - Microsoft
---

# [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/) ✅

Given two strings `s` and `t`, return `true` _if_ `t` _is an anagram of_ `s`_, and_ `false` _otherwise_.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

```
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**

```
Input: s = "rat", t = "car"
Output: false
```

**Constraints:**

- `1 <= s.length, t.length <= 5 * 104`
- `s` and `t` consist of lowercase English letters.

**Follow up:** What if the inputs contain Unicode characters? How would you adapt your solution to such a case?

**Solution:**

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        Map<Character, Integer> map = new HashMap<Character, Integer>();

        char[] chS = s.toCharArray();
        for (int i = 0; i < chS.length; i++){
            map.put(chS[i], map.getOrDefault(chS[i], 0) + 1);
        }

        char[] chT = t.toCharArray();
        for (int j = 0; j < chT.length; j++){
            if (!map.containsKey(chT[j]) || map.get(chT[j]) == 0){
                return false;
            }

            map.put(chT[j], map.get(chT[j]) - 1);
        }

        List<Integer> values = new ArrayList<Integer>(map.values());

        for (int i = 0; i < map.size(); i++){
            if (values.get(i) != 0){
                return false;
            }
        }

        return true;
    }
}

// TC: O(n)
// sC: O(n)
```

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()){
            return false;
        }

        int[] count = new int[26];

        for (int i = 0; i < s.length(); i++){
            count[s.charAt(i) - 'a'] = count[s.charAt(i) - 'a'] + 1;
        }

        for (int i = 0; i < t.length(); i++){
            if (count[t.charAt(i) - 'a'] == 0){
                return false;
            }
            count[s.charAt(i) - 'a'] = count[s.charAt(i) - 'a'] - 1;
        }

        return true;
    }
}

// TC: O(n)
// SC: O(1)
```



```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()){
            return false;
        }
        Map<Character, Integer> map = new HashMap<>();

        for (int i = 0; i < s.length(); i++){
            char cur = s.charAt(i);
            map.put(cur, map.getOrDefault(cur, 0) + 1);
        }

        for (int i = 0; i < t.length(); i++){
            char cur = t.charAt(i);
            if (!map.containsKey(cur)){
                return false;
            }else{
                int count = map.get(cur);
                if (count < 1){
                    return false;
                }
                map.put(cur, count - 1);
            }
        }

        return true;
    }
}

// TC: O(n)
// SC: O(n)
```





Earth -> Heart
**Listen** -> Silent



input: string, string

output: boolean 

```java
// Map<Character, Integer> map =  Earth
//.   E        1
//    H        0
// to check Heart       
// whether match the map 
// if match the characters and Integer numbers in the map -> return true'
// if (not ){}// false
// TC: O(n)
// SC: O(n)
// Heart
// j
// Earth -> Heart

// listens -> silent -> false
public boolean solution(String one, String two){
    // base case
  	if (one.length() != two.length()){
      return false;
    }
  
	  Map<Character, Integer> map = new HashMap<>();
  	// here
    //  earth 
  //   e    0 
  //   a   0
  //   r   0 
  //   h   0
  //   t  0
  // ---------
  
  // listens
  // s  1

  	int n = one.length();
  	for (int i = 0; i < n; i++){
      map.put(one.charAt(i), map.getOrDefault(one.charAt(i), 0) + 1);
    }
  
    // match part
  // H eart
  //       j 
  // silent
 //       j 
  	int m = two.length(); 
  	for (int j = 0; j < m; j++){
      // match
      char curTwo = two.charAt(j); //h 
      // compare in the map
      // 1. map not exit the char
      if (!map.containsKey(curTwo)){
        return false;
      }else{
        // map. contiansKey() // count >= 0
       	int curCount = map.get(curTwo); // 1
        if (curCount <= 0){
          return false;
        }else{
          // curCount > 1
          map.put(curTwo, curCount - 1);
          if (map.get(curTwo) == 0){
            map.remove(cutTwo);
          }
        }
      }
    }
  	if (map.size() == 0){
      return true;
    }
  	return false;
  	
}
```

