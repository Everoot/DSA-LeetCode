---
tags:
    - Two Pointers
    - String
    - Top Interview 150
---

# [151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in `s` will be separated by at least one space.

Return *a string of the words in reverse order concatenated by a single space.*

**Note** that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

 

**Example 1:**

```
Input: s = "the sky is blue"
Output: "blue is sky the"
```

**Example 2:**

```
Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
```

**Example 3:**

```
Input: s = "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
```

**Solution:**

```java
class Solution {
    public String reverseWords(String s) {
        if (s == null || s.length() <= 1){
            return s;
        }

        char[] array = s.toCharArray();

        int n = array.length;

        reverse(array, 0, n - 1);

        reverseEachWord(array);

        return cleanSpace(array);
    }

    private void reverse(char[] a, int l, int r){
        while(l < r){
            char t = a[l];
            a[l] = a[r];
            a[r] = t;
            l++;
            r--;
        }
    }

    private void reverseEachWord(char[] a){
        int n = a.length;

        int i = 0;
        int j = 0;

        while(i < n){
            // 跳过空格, 找到单词起点
            while(i < n && a[i] == ' '){
                i++;
            }

            // 保护: 防止越界
            if (i >= n){
                break;
            }

            // j 从 i开始, 走到单词末尾后一位 (即空格或n)
            j = i;
            while(j < n && a[j] != ' '){
                j++;
            }

            // 反转当前单词 [i, j-1]
            reverse(a, i, j - 1);

            // 进入下一段
            i = j;
        }
    }

    private static String cleanSpace(char[] a){
        int n = a.length;
        int slow = 0;
        int fast = 0;

        while(fast < n){
            // 跳过多余空格j 
            while(fast < n && a[fast] == ' '){
                fast++;
            }

            // 拷贝单词
            while(fast < n && a[fast] != ' '){
                a[slow] = a[fast];
                slow++;
                fast++;
            }

            // 跳过单词后面的空格
            while(fast < n && a[fast] == ' '){
                fast++;
            }

            if (fast < n){
                a[slow] = ' ';
                slow++;
            }
        }

        return new String(a, 0, slow);
    }
}
// TC: O(n)
// SC: O(n)
```



```java
class Solution {
    public String reverseWords(String s) {
      // base case
      if (s == null || s.length() <= 1){
        return s;
      }
      // try to convert it to char array and
      // solve the problem in-place.
      char[] array = s.toCharArray();
      StringBuilder sb = new StringBuilder();

      // 1. reverse the whole char array.
      //  " hello  world  "=> " dlrow  olleh "
      //                        l     i   
      reverse(array, 0, array.length - 1);
    
      // 2. reverse each word
      reverseWords(array);
      
      // 3. clean up space
      return cleanSpaces(array);
    }

    // 1. reverse whole string
    private static void reverse(char[] array, int left, int right){
      while(left < right){
        swap(array, left, right);
        left++;
        right--;
      }
    }

    private static void swap(char[] array, int left, int right){
      char rem = array[left];
      array[left] = array[right];
      array[right] = rem;
    }

    // 2. reverse each word
    private static void reverseWords(char[] array){
      int left = 0;
      int right = 0;

      while(left < array.length){
        while(left < right || left < array.length && array[left] == ' '){
          left++;
          // skip spaces;
        }

        while(right < left || right < array.length && array[right] != ' '){
          right++;
          // skip non spaces
        }

        reverse(array, left, right-1); // reverse the word.
      }

    }

    private static String cleanSpaces(char[] array){
      int slow = 0;
      int fast = 0;
      while(fast < array.length){
        while(fast < array.length && array[fast] == ' '){
          fast++;
        }

        while(fast < array.length && array[fast] != ' '){
          array[slow] = array[fast];
          slow++;
          fast++;
        }

        while(fast < array.length && array[fast] == ' '){
          fast++;
        }

        if (fast < array.length){
          array[slow] = ' '; 
          slow++;
        }
      }

      return new String(array, 0, slow);
    }
}
```





```java
class Solution {
    public String reverseWords(String s) {
      // base case
      if (s == null || s.length() <= 1){
        return s;
      }
      // try to convert it to char array and
      // solve the problem in-place.
      char[] array = s.toCharArray();
      StringBuilder sb = new StringBuilder();

      // 1. reverse the whole char array.
      //  " hello  world  "=> " dlrow  olleh "
      //                        l     i   
      reverse(array, 0, array.length - 1);
    
      // 2. reverse each word
      reverseEachWords(array);
      
      // 3. clean up space
      return cleanSpaces(array);
    }

    // 1. reverse whole string
    private static void reverse(char[] array, int left, int right){
      while(left < right){
        swap(array, left, right);
        left++;
        right--;                      
      }
    }

    private static void swap(char[] array, int left, int right){
      char rem = array[left];
      array[left] = array[right];
      array[right] = rem;
    }

    // 2. reverse each word
    // " dlrow  olleh "
    //   l
    //  r
    private static void reverseEachWords(char[] array){
      int left = 0;
      int right = 0;

      while(left < array.length){
        while(left < array.length && array[left] == ' '){
          left++;// skip leading spaces before every word
        }

        while(right < left && array[right] == ' '){
          right++;// points right to the same position as left, because left has already
          // skipped the leading spaces in prior while
        }

        while(right < array.length && array[right] != ' '){
          right++; 
          // find the whole word starts from left, ends at right unitil array[right] is ' '
        }
        reverse(array, left, right-1); // reverse the word.
        
        while(left < right && array[left] != ' '){
          left++;
          // left is the start index of last word, right - 1 is the end of the last word, points to the same position as j, then start finding the next word.
        }


      }

    }

    private static String cleanSpaces(char[] array){
      int slow = 0;
      int fast = 0;
      while(fast < array.length){
        while(fast < array.length && array[fast] == ' '){
          fast++;
        }

        while(fast < array.length && array[fast] != ' '){
          array[slow] = array[fast];
          slow++;
          fast++;
        }

        while(fast < array.length && array[fast] == ' '){
          fast++;
        }

        if (fast < array.length){
          array[slow] = ' '; 
          slow++;
        }
      }

      return new String(array, 0, slow);
    }
}
```





```java
class Solution {
    public String reverseWords(String s) {
      // remove leading spaces
      s = s.trim();
      // split by multiple space
      List<String> wordList = Arrays.asList(s.split("\\s+"));
      Collections.reverse(wordList);
      return String.join(" ", wordList);
        
    }
}

/*
It simply means: slice the input string s on the given regular expression.

That regular expression simply says: "one or more whitespaces". (see here for an extensive descriptions what those patterns mean)

Thus: that call to split returns an array with "john", "fitzgerald", ... That array is directly "processed" using the for-each type of for loops.

*/
```





```java
class Solution {
    public String reverseWords(String s) {
        // Step 1: Remove leading and trailing spaces
        s = s.trim();
        
        // Step 2: Split by one or more spaces
        String[] words = s.split("\\s+");
        
        // Step 3: Reverse the words
        StringBuilder sb = new StringBuilder();
        for (int i = words.length - 1; i >= 0; i--) {
            sb.append(words[i]);
            if (i > 0) sb.append(" ");
        }
        
        // Step 4: Return the result
        return sb.toString();
    }
}

// TC: O(n)
// SC: O(n)
```

