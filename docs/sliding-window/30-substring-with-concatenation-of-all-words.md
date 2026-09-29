---
tags:
    - Hash Table
    - String
    - Sliding Window
    - Top Interviews
---

# [30. Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)

You are given a string `s` and an array of strings `words`. All the strings of `words` are of **the same length**.

A **concatenated string** is a string that exactly contains all the strings of any permutation of `words` concatenated.

- For example, if `words = ["ab","cd","ef"]`, then `"abcdef"`, `"abefcd"`, `"cdabef"`, `"cdefab"`, `"efabcd"`, and `"efcdab"` are all concatenated strings. `"acdbef"` is not a concatenated string because it is not the concatenation of any permutation of `words`.

Return an array of *the starting indices* of all the concatenated substrings in `s`. You can return the answer in **any order**.

 

**Example 1:**

**Input:** s = "barfoothefoobarman", words = ["foo","bar"]

**Output:** [0,9]

**Explanation:**

The substring starting at 0 is `"barfoo"`. It is the concatenation of `["bar","foo"]` which is a permutation of `words`.
The substring starting at 9 is `"foobar"`. It is the concatenation of `["foo","bar"]` which is a permutation of `words`.

**Example 2:**

**Input:** s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]

**Output:** []

**Explanation:**

There is no concatenated substring.

**Example 3:**

**Input:** s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]

**Output:** [6,9,12]

**Explanation:**

The substring starting at 6 is `"foobarthe"`. It is the concatenation of `["foo","bar","the"]`.
The substring starting at 9 is `"barthefoo"`. It is the concatenation of `["bar","the","foo"]`.
The substring starting at 12 is `"thefoobar"`. It is the concatenation of `["the","foo","bar"]`.

 

**Constraints:**

- `1 <= s.length <= 104`
- `1 <= words.length <= 5000`
- `1 <= words[i].length <= 30`
- `s` and `words[i]` consist of lowercase English letters.



**Solution:**

```java
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> result = new ArrayList<>();

        if (s.length() == 0 || words.length == 0){
            return result;
        }

        int wordLen = words[0].length();
        int wordCount = words.length;
        
        int windowSize = wordLen * wordCount;

        Map<String, Integer> wordMap = new HashMap<>();

        for (String word : words){
            wordMap.put(word, wordMap.getOrDefault(word, 0) + 1);
        }
				// 枚举第一个窗口的左端点，做 wordLen 次起点不同的滑动窗口
        for (int i = 0; i < wordLen; i++){
            int left = i;
            int right = i;

            int count = 0;

            Map<String, Integer> windowMap = new HashMap<>();

            while(right + wordLen <= s.length()){
                String word = s.substring(right, right + wordLen);

                right = right + wordLen;

                if (wordMap.containsKey(word)){
                    windowMap.put(word, windowMap.getOrDefault(word, 0) + 1);
                    count++;

                    while(windowMap.get(word) > wordMap.get(word)){

                        String leftWord = s.substring(left, left + wordLen);

                        windowMap.put(leftWord, windowMap.get(leftWord) - 1);

                        left = left + wordLen;
                        count--;
                    }

                    if (count == wordCount){
                        result.add(left);
                    }
                }else{
                    windowMap.clear();
                    count = 0;
                    left = right;
                }
            }
        }

        return result;
    }
}

// TC: O(n)
// SC: O(k)
```





[1456. Maximum Number of Vowels in a Substring of Given Length](https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)

[567. Permutation in String](https://leetcode.com/problems/permutation-in-string/)
