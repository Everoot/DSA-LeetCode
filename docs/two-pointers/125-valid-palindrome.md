---
tags:
    - Two Pointers
    - String
    - Top Interviews
---

# [125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` *if it is a **palindrome**, or* `false` *otherwise*.

 

**Example 1:**

```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

**Example 2:**

```
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

**Example 3:**

```
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
```

 

**Constraints:**

- `1 <= s.length <= 2 * 105`
- `s` consists only of printable ASCII characters.



**Solution:**

```java
class Solution {
    public boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() - 1;
        while(left <= right){
            char curLeft = Character.toLowerCase(s.charAt(left));
            char curRight = Character.toLowerCase(s.charAt(right));

            if (curLeft == ' ' || (Character.isAlphabetic(curLeft) == false && Character.isDigit(curLeft) == false)){
                left++;
                continue;
            }

            if (curRight == ' ' || (Character.isAlphabetic(curRight) == false && Character.isDigit(curRight) == false)){
                right--;
                continue;
            }

            if (curLeft == curRight){
                left++;
                right--;
            }else if (curLeft != curRight){
                return false;
            }

            

        }

        return true;
    }
}

// TC: O(n)
// SC: O(1)
```



```java
class Solution {
    public boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() -1;
        while(left <= right){
          char cl = s.charAt(left);
          char cr = s.charAt(right);
          if (cl >= 'A' && cl <= 'Z'){
              cl = (char) (32 + cl);
          }

          if (cr >= 'A' && cr <= 'Z'){
              cr = (char) (32 + cr);
          }

          if (!((cl >= 'a' && cl <= 'z') || ( cl >= '0' && cl <= '9'))){
              left++;
              continue;
          }

          if (!((cr >= 'a' && cr <= 'z') || (cr >= '0' && cr <= '9'))){
              right--;
              continue;
          }

          if (cl != cr){
              return false;
          }
          left++;
          right--;
        }

        return true;
    }
}
// TC: O(n)
// SC: O(1)
```



```java
class Solution {
    public boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() -1;
        while(left < right){
            if (!Character.isLetterOrDigit(s.charAt(left))){
                left++;
            }else if (!Character.isLetterOrDigit(s.charAt(right))){
                right--;
            }else {
                if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))){
                    return false;
                }
                left++;
                right--;
            }
        }

        return true;
    }
}

// TC: O(n)
// SC: O(1)
```



````java
class Solution {
    public boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() - 1;

        while(left < right){
            char curLeft = s.charAt(left);
            char curRight = s.charAt(right);

            if (!Character.isLetterOrDigit(curLeft)){
                left++;
                continue;
            }else if (!Character.isLetterOrDigit(curRight)){
                right--;
                continue;
            }

            if (Character.toLowerCase(curLeft) == Character.toLowerCase(curRight)){
                right--;
                left++;
            }else{
                return false;
            }

        }

        return true;
    }
}

// TC: O(n)
// SC: O(1)
````

