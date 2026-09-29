---
tags: 
    - Hash Table
    - String
    - Sliding Window
    - Top Interviews
---

# [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

Given two strings `s` and `t` of lengths `m` and `n` respectively, return *the **minimum window substring **

 *of* `s` *such that every character in* `t` *(**including duplicates**) is included in the window*. If there is no such substring, return *the empty string* `""`.



The testcases will be generated such that the answer is **unique**.

 

**Example 1:**

```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```

**Example 2:**

```
Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window.
```

**Example 3:**

```
Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.
```

 

**Constraints:**

- `m == s.length`
- `n == t.length`
- `1 <= m, n <= 105`
- `s` and `t` consist of uppercase and lowercase English letters.

 

**Follow up:** Could you find an algorithm that runs in `O(m + n)` time?



**Solution:**

```java
class Solution {
    public String minWindow(String s, String t) {
        int n = s.length();
        int m = t.length();

        if (n < m){
            return "";
        }

        Map<Character, Integer> map = new HashMap<>();

        for (int i = 0; i < t.length(); i++){
            map.put(t.charAt(i), map.getOrDefault(t.charAt(i), 0) + 1);
        }


        int left = 0;
        int count = m;
        int resultLeft = 0;
        int resultRight = 0;

        int minLength = Integer.MAX_VALUE;

        for (int right = 0; right < n; right++){
            char curRight = s.charAt(right);
            if (map.containsKey(curRight)){
                int curRightCount = map.get(curRight);

                if (curRightCount > 0){
                    count--;
                }

                map.put(curRight, curRightCount - 1);
            }

            while(count == 0){
                int curLen = right - left + 1;
                if (curLen < minLength){
                    minLength = curLen;
                    resultLeft = left;
                    resultRight = right;
                }

                if (!map.containsKey(s.charAt(left))){
                    left++;
                }else{
                    if (map.get(s.charAt(left)) == 0){
                        count++;
                    }

                    map.put(s.charAt(left), map.get(s.charAt(left)) + 1);
                    left++;
                }
            }
        }

        if (minLength == Integer.MAX_VALUE){
            return "";
        }else{
            return s.substring(resultLeft, resultRight + 1);
        }
    }
}
// TC: O(n)
// SC: O(n)
```



```java
class Solution {
    public String minWindow(String s, String t) {
        // Treat 's' as the list of food items in a supermarket (the available shelf)
        // Treat 't' as the shopping list (items you need to pick up and take home)

        if (s.length() < t.length()){
            return "";
        }
        // If the supermarket has fewer items than you need, return home directly — it's impossible to fulfill the list.

        // We'll use a sliding window [left, right) to scan through 's' and find the shortest window that contains all characters in 't' (including duplicates)

        // A D O B E C O D E B A N C             
        // l
        //     r

        // A B C
        // 

        // I need use [l, r] to scan whether ABC in current window

        // Count the required number of each character in 't'
        Map<Character, Integer> map = new HashMap<>();
        for (int i = 0; i < t.length(); i++){ // TC: O(n)
            map.put(t.charAt(i), map.getOrDefault(t.charAt(i), 0) + 1);
        }


        // Define the sliding window boundaries
        int left = 0;  // include
        int right = 0; // include
        
        int need = t.length(); // total number of characters we still need to collect

        int startInd = 0;  // include, the starting index of the minimum window
        int minLength = Integer.MAX_VALUE; // track the minimum length

        while(right < s.length()){ // TC: O(m)
            char curRight = s.charAt(right);
            if (map.containsKey(curRight)){
                // this product is what we need, we need to update the need count
                int curRightCount = map.get(curRight);
                // this product count how much we need
                if (curRightCount > 0){
                    // This is a needed character and we haven’t fully collected it yet
                    need--; // current one we got one of the product should update the need
                }
                map.put(curRight, curRightCount - 1);
                // Decrease the count in the map to reflect that we’ve taken one
            }


            // Once we have collected all required characters, try to shrink the window
            while(need == 0){ // move left        
                // make sure window has need.size() TC: O(n + m - need.size())
                // Update the minimum window if this one is smaller
                if (right - left + 1 <= minLength){
                    // 0 1 2 3 4 5 6 7 8 9 10 11 12
                    // A D O B E C O D E B A  N  C 
                    //     r
                    // l
                    // [   ]  include
                    // ADO -> l-r -> len = 3
                    // 2 - 0 + 1 = 3 
                    minLength = right - left + 1;
                    startInd = left;// update left -> we short the window
                    
                }


                if (!map.containsKey(s.charAt(left))){
                    // Not a needed character — just move the left pointer
                    left++;
                }else{
                    // A needed character is being removed — return it back to the map
                    if (map.get(s.charAt(left)) == 0){
                        // This character was contributing to a valid window; we now lose it
                        need++; 
                    }

                    // maintain the map
                    map.put(s.charAt(left), map.get(s.charAt(left)) + 1);
                    left++;
                }
            }

            right++; // Move the right pointer to expand the window

            // Overall time complexity is < 2m since each character is processed at most twice
        }

        if (minLength == Integer.MAX_VALUE){
            // No valid window found
            return "";
        }else{
            // Return the shortest valid window
            return s.substring(startInd, startInd + minLength);
            // Note: substring is [startInd, startInd + minLength)
            // s.substring  [ )  
            //                     
            // 0 1 2 3 4 5 6 7 8 9 10 11 12
            // A D O B E C O D E B A  N  C 
            //     r
            // l
            // s
            // [   ]  include
            // ADO -> l-r -> len = 3
            // 2 - 0 + 1 = 3 
            // minLength = 3
            // 0 + 3 = 3 
            // [0 3) = [0, 2] -> ADO 

        }
    }
}

// TC: O(m + n)
// SC: O(n)    O(128) -> O(1) 
```



```java
class Solution {
    public String minWindow(String s, String t) {
        if (s.length() < t.length()){
            return "";
        }

        Map<Character, Integer> map = new HashMap<>();

        for (int i = 0; i < t.length(); i++){
            map.put(t.charAt(i), map.getOrDefault(t.charAt(i), 0) + 1);
        }

        int slow = 0;
        int count = t.length();
        int startInd = 0;
        int minLength = Integer.MAX_VALUE;

        for (int fast = 0; fast < s.length(); fast++){
            char curFast = s.charAt(fast);

            if (map.containsKey(curFast)){
                int curFastCount = map.get(curFast);
                if (curFastCount > 0){
                    count--;
                }

                map.put(curFast, curFastCount - 1);
            }

            while(count == 0){
                if (fast - slow + 1 <= minLength){
                    minLength = fast - slow + 1;
                    startInd = slow;
                    
                }

                if (!map.containsKey(s.charAt(slow))){
                    slow++;
                }else{
                    if (map.get(s.charAt(slow)) == 0){
                        count++;
                    }

                    map.put(s.charAt(slow), map.get(s.charAt(slow)) + 1);
                    slow++;
                }
            }
        }

        if (minLength == Integer.MAX_VALUE){
            return "";
        }else{
            return s.substring(startInd, startInd + minLength);
        }
    }
}

// TC: O(m + n)
// SC: O(n) -> O(1) 
```



```java
class Solution {
    public String minWindow(String s, String t) {
        if (s.length() < t.length()){
            return "";
        }

        Map<Character, Integer> map = new HashMap<Character, Integer>();

        int slow = 0;
        int fast = 0;
        int minLength = Integer.MAX_VALUE;
        int count = t.length();
        int startInd = 0;

        for (int i = 0; i < t.length(); i++){
            map.put(t.charAt(i), map.getOrDefault(t.charAt(i), 0) + 1);
        }

        while(fast < s.length()){
            char curFast = s.charAt(fast);
            if (map.containsKey(curFast)){
                int curFastCount = map.get(curFast);
                if (curFastCount > 0){
                    count--;
                }

                map.put(curFast, curFastCount - 1);
            }

            fast++;

            while(count == 0){

                if (fast - slow < minLength){
                    minLength = fast - slow;
                    startInd = slow;
                }

                char curSlow = s.charAt(slow);
                if (!map.containsKey(curSlow)){
                    slow++;
                }else {
                    // contains
                    int curSlowCount = map.get(curSlow);
                    if (curSlowCount == 0){
                        count++;
                    }
                    map.put(curSlow, curSlowCount + 1);

                    slow++;
                }
            }
        }

        if (minLength == Integer.MAX_VALUE){
            return "";
        }else{
            return s.substring(startInd, startInd + minLength);
        }
    }
}
```



```java
class Solution {
    public String minWindow(String s, String t) {
        if (t.length() > s.length()){
            return "";
        }

        Map<Character, Integer> freq = new HashMap<Character, Integer>();

        for (int i = 0; i < t.length(); i++){
            freq.put(t.charAt(i), freq.getOrDefault(t.charAt(i), 0) + 1);
        }

        int slow = 0;
        int fast = 0; 
        int count = t.length();
        int minLength = Integer.MAX_VALUE;

        int startInd = 0;

        while(fast < s.length()){
            //   // 每次遇到t中的字符，减少需要匹配的数量
            if (freq.containsKey(s.charAt(fast))){
                if (freq.get(s.charAt(fast)) > 0){
                    
                    count--;
                }
                freq.put(s.charAt(fast), freq.get(s.charAt(fast)) - 1);

            }
            

            fast++;

            while(count == 0){
                if (fast - slow <= minLength){
                    minLength = fast - slow;
                    startInd = slow;
                }

                if (!freq.containsKey(s.charAt(slow))){
                    slow++;
                }else{
                    if (freq.get(s.charAt(slow)) == 0){
                        count++;
                    }
                    freq.put(s.charAt(slow), freq.get(s.charAt(slow)) + 1);

                    slow++;
                }
              
// 为什么要增加 count？因为当频率为 0 时，表示当前窗口已经完全包含了 t 中的所有该字符。如果我们右移 slow 指针，将这个字符移出窗口，意味着我们不再满足窗口内的完整性，所以我们需要增加 count，表示还需要一个这样的字符来恢复完整性
            }

        }

        if (minLength == Integer.MAX_VALUE){
            return "";
        } else {
            return s.substring(startInd, startInd + minLength);
        }
    }
}

// TC: O(n)
// SC: O(n)

/*
        s = "ADOBECODEBANC", t = "ABC"  Map<Character, Integer>  freq   
             s
              f 


        count 1


        A : 1
        B : 1
        C : 1 


 */
```

