---
 tags:
     - Array
     - String
     - Simulation
     - Top Interviews
---

# [68. Text Justification](https://leetcode.com/problems/text-justification/)

Given an array of strings `words` and a width `maxWidth`, format the text such that each line has exactly `maxWidth` characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces `' '`when necessary so that each line has exactly `maxWidth` characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line does not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left-justified, and no extra space is inserted between words.

**Note:**

- A word is defined as a character sequence consisting of non-space characters only.
- Each word's length is guaranteed to be greater than `0` and not exceed `maxWidth`.
- The input array `words` contains at least one word.

 

**Example 1:**

```
Input: words = ["This", "is", "an", "example", "of", "text", "justification."], maxWidth = 16
Output:
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```

**Example 2:**

```
Input: words = ["What","must","be","acknowledgment","shall","be"], maxWidth = 16
Output:
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]
Explanation: Note that the last line is "shall be    " instead of "shall     be", because the last line must be left-justified instead of fully-justified.
Note that the second line is also left-justified because it contains only one word.
```

**Example 3:**

```
Input: words = ["Science","is","what","we","understand","well","enough","to","explain","to","a","computer.","Art","is","everything","else","we","do"], maxWidth = 20
Output:
[
  "Science  is  what we",
  "understand      well",
  "enough to explain to",
  "a  computer.  Art is",
  "everything  else  we",
  "do                  "
]
```

 

**Constraints:**

- `1 <= words.length <= 300`
- `1 <= words[i].length <= 20`
- `words[i]` consists of only English letters and symbols.
- `1 <= maxWidth <= 100`
- `words[i].length <= maxWidth`



Solution:

```java
class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        List<String> result = new ArrayList<>();

        int n = words.length;

        int i = 0;

        while(i < n){
            int j = i;

            int lineLen = 0;
        // j < 7 && 0 + 4 + (0-0) = 4 <= 16
        // j = 1 < 7 && line + 2 + (1 -0) = 4 + 2 + 1 = 7 <= 16
        // j = 2 < 7 && 7 + 2 +(2 -0) = 9 + 2 = 11 <= 16
        // j = 3 <. 7 && 11 + 7 + (3 - 0) = 11 + 7  + 3 > 16   xx
            while(j < words.length && lineLen + words[j].length() + (j - i) <= maxWidth){
                lineLen = lineLen + words[j].length();
                j++;
            }
            // j = 2, len = 11

            int wordCount = j - i; // 3 - 0 = 3
            int spaces = maxWidth - lineLen; // 16 - 11 = 5

            StringBuilder sb = new StringBuilder();

            if (j == words.length || wordCount == 1){
                for (int k = i; k < j; k++){
                    sb.append(words[k]);
                    if (k < j - 1){
                        sb.append(" "); //最后一个单词后面不能加空格, 只在单词间加
                    }
                }

                int remaining = maxWidth - sb.length();
                while(remaining > 0){
                    sb.append(" "); 
                    remaining--;
                }
            }else{
                int slots = wordCount - 1; // 3 - 1 = 2 
                int each = spaces / slots; // 5 / 2 = 2
                int extra = spaces % slots; // 5 % 2 = 1 // 多出来的空格，要给左边的 gap
                // 0 
                for (int k = i; k < j; k++){
                    sb.append(words[k]); // This // is 

                    if (k < j - 1){ // 0 < 3 -1 = 0 < 2 // 1 < 3 -1 = 1 < 2 
                        int spaceCount = each + (k - i < extra ? 1 : 0); 
                        //               2 + (0 - 0) < 1 = 1 =3  
                                      // 2 + (1 - 0)  = 0
                        // (k - i) 当前处理的是第几个 gap（从左往右编号）

                        while (spaceCount > 0){
                            spaceCount--;
                            sb.append(" ");
                        }
                    }
                    // This_ _ _ 
                }
            }

            result.add(sb.toString());
            i = j;
        }

        return result;
    }
}
// Time complexity: O(nk)
// SC: O(m)
```



```java
import java.util.*;

class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        List<String> ans = new ArrayList<>();
        int n = words.length;

        for (int i = 0; i < n; ) {
            int start = i;                    // 本行第一个单词下标
            int sumLen = words[i].length();   // 最小长度：先放第一个单词

            // 内层：尽可能多装单词（从第二个单词开始，每个单词前至少 1 个空格）
            for (i++; i < n && sumLen + 1 + words[i].length() <= maxWidth; i++) {
                sumLen += 1 + words[i].length();
            }

            int extraSpaces = maxWidth - sumLen; // 额外需要补的空格（不含每 gap 至少 1 个空格）
            int gaps = i - start - 1;            // gap 数 = 单词数 - 1

            // 特殊：只有一个单词 or 最后一行 => 左对齐
            if (gaps == 0 || i == n) {
                StringBuilder row = join(words, start, i, " ");
                row.append(" ".repeat(extraSpaces)); // 末尾补齐
                ans.add(row.toString());
                continue;
            }

            // 一般：两端对齐，把 extraSpaces 均分到 gaps，左边多
            int avg = extraSpaces / gaps; // 每个 gap 额外补 avg 个
            int rem = extraSpaces % gaps; // 前 rem 个 gap 再多补 1 个

            StringBuilder row = new StringBuilder();
            row.append(words[start]);

            for (int k = start + 1; k < i; k++) {
                int gapIndex = k - (start + 1); // 第几个 gap（0-based）
                int spaceCount = 1 + avg + (gapIndex < rem ? 1 : 0); // 1 是每 gap 必须的空格
                row.append(" ".repeat(spaceCount)).append(words[k]);
            }

            ans.add(row.toString());
        }

        return ans;
    }

    private StringBuilder join(String[] words, int start, int end, String sep) {
        StringBuilder res = new StringBuilder();
        for (int i = start; i < end; i++) {
            if (i > start) res.append(sep);
            res.append(words[i]);
        }
        return res;
    }
}
```

