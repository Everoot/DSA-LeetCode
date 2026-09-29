---
tags:
    - Two Pointers
    - String
    - Dynamic Programming
    - Top Interviews
---

# [5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

Given a string `s`, return *the longest* *palindromic substring* in `s`.

**Example 1:**

```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

**Example 2:**

```
Input: s = "cbbd"
Output: "bb"
```

 

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consist of only digits and English letters.



**Solution:**



## **Method 1: Center Expansion**

The most brute-force approach is to enumerate all substrings and then check whether each substring is a palindrome. Since there are $O(n^2)$ substrings, and checking whether each substring is a palindrome takes $O(n)$ time, the overall time complexity is $O(n^3)$, which is too slow.

Can we determine whether a string is a palindrome in $O(1)$ time?

For example, consider the substring `abcba`. The leftmost and rightmost characters are both `a`. If the middle substring `bcb` is a palindrome, then we can know in $O(1)$ time that `abcba` is also a palindrome.
 For the substring `bcb`, the leftmost and rightmost characters are both `b`. If the middle character `c` is a palindrome, then we can know in $O(1)$ time that `bcb` is a palindrome.
 Clearly, `c` itself is a palindrome, so `bcb` is a palindrome, and therefore `abcba` is also a palindrome.

Given this observation, why not start directly from `c` and expand outward?

- `c` is a palindrome.
- Check whether the characters on the left and right of `c` are equal. If they are, then `bcb` is a palindrome.
- Continue expanding outward. If the characters on the left and right of `bcb` are equal, then `abcba` is a palindrome.

In this way, we can determine whether a substring is a palindrome in $O(1)$ time by expansion.
 All these palindromes have **odd length**, and we call them **odd-length palindromes**.

Palindromes can also have **even length**, which we call **even-length palindromes**.

For example, consider the substring `abccba`. We can start from the middle `"cc"`:

- `"cc"` is a palindrome.
- Check whether the characters on the left and right of `"cc"` are equal. If they are, then `"bccb"` is a palindrome.
- Continue expanding outward. If the characters on the left and right of `"bccb"` are equal, then `"abccba"` is a palindrome.

In general, we enumerate `i = 0, 1, 2, …, n − 1` as the center of **odd-length palindromes**, and expand to both sides:

- Initialize `l = r = i`.
- If `s[l] == s[r]`, expand outward by decrementing `l` and incrementing `r`, and continue checking whether the longer substring is a palindrome, until the indices go out of bounds or `s[l] != s[r]`.
- When the loop ends, the substring from `s[l + 1]` to `s[r − 1]` is a palindrome.
   If its length `r − l − 1` is greater than the current best answer, update the answer’s left and right boundaries to `l + 1` and `r − 1`, which makes it easy to output the actual substring.

Similarly, enumerate `i` and `i + 1` as the center of **even-length palindromes**, that is, initialize `l = i` and `r = i + 1`. The rest of the process is the same.



```java
class Solution {
    public String longestPalindrome(String s) {
       char[] sChar = s.toCharArray();

       int n = sChar.length;

       int ansLeft = 0;
       int ansRight = 0;
       //    0 1 2 3 4
       //    b a b a d
       //      i
       //   l      r
       // 

       for(int i = 0; i < n; i++){
            int l = i;
            int r = i;
            while(l >= 0 && r < n && sChar[l] == sChar[r]){
                l--;
                r++;
            }

            if ((r-1) - (l + 1) + 1 > ansRight - ansLeft + 1){
                ansLeft = l + 1;
                ansRight = r - 1;
            }
       }

       for (int i = 0; i < n - 1; i++){
        int l = i;
        int r = i + 1;
        while(l >= 0 && r < n && sChar[l] == sChar[r]){
            l--;
            r++;
        }

        if ((r - 1) - (l + 1) + 1 > ansRight - ansLeft + 1){
            ansLeft = l + 1;
            ansRight = r - 1;
        }
       }


       return s.substring(ansLeft, ansRight + 1);
    }
}
```





```java
class Solution {
    public String longestPalindrome(String s) {
        char[] sChar = s.toCharArray();

        int n = s.length();
        int ansLeft = 0;
        int ansRight = 0;


        // odd
        // 0 1 2 3
        // b a b a d
        //l
        //       r
        // 3 - (-1) = 4 -1

        for (int i = 0; i < n; i++){
            int l = i;
            int r = i;

            while(l >= 0 && r < n && sChar[l] == sChar[r]){
                l--;
                r++;
            }

            if (r - l - 1 > ansRight - ansLeft + 1){
                ansLeft = l + 1;
                ansRight = r - 1;
            }

        }

        // even
        // c b b b
        //   i
        //      j

        for (int i = 0; i < n - 1; i++){
            int l = i;
            int r = i + 1;
            while(l >= 0 && r < n && sChar[l] == sChar[r]){
                l--;
                r++;
            }

            if (r - l - 1 > ansRight - ansLeft + 1){
                ansLeft = l + 1;
                ansRight = r - 1;
            }
        }

        return s.substring(ansLeft, ansRight + 1);
    }
}
```









|      | i    | 0            | 1           | 2          | 3         | 4        |
| ---- | ---- | ------------ | ----------- | ---------- | --------- | -------- |
| j    |      | b            | a           | b          | a         | d        |
| 0    | b    | (0,0) b T    | (1,0) x     |            |           |          |
| 1    | a    | (0,1)ba F    | (1,1) a T   |            |           |          |
| 2    | b    | (0,2)bab T   | (1,2)ab F   | (2,2) b T  |           |          |
| 3    | a    | (0,3)baba F  | (1,3)aba T  | (2,3)ba F  | (3,3)a T  |          |
| 4    | d    | (0,4)babad F | (1,4)abad F | (2,4)bad F | (3,4)ad F | (4,4)d T |



很有意思的题目 ??????        <- 年少留下些什么评语???! 况且也不年少了.

```java
class Solution {
    public String longestPalindrome(String s) {
        for (int len = s.length(); len > 0; len--){// check current len wether avaliable
            for (int start = 0; start <= s.length() - len; start++){
                // 0 , start <= start <= 5 - 4 = 1    start: [0, 1]
                // 0, start <= start <= 5 - 3 = 2     start:[0, 1, 2]
                // start <= s.length() - len make sure check the current len
                if (check(start, start + len, s)){
                    // 
                    // 0, 5, babad
                    return s.substring(start, start + len);
                }
            }
        }

        return "";
        
    }

    private boolean check(int i, int j, String s){
        int left = i;
        int right = j-1;

        while(left < right){
            if (s.charAt(left) != s.charAt(right)){
                return false;
            }

            left++;
            right--;
        }

        return true;
    }
}


// TC: O(n^3)
// SC: O(1)

/*

        b a b  a d
        l        r 
        0   end            b a b
          1    end          a b a 
            2   end          ba d

        s     len - 1         b a b a d 
                              s     

        b a b a d 
        s      end 



        b a b a d
         s    end

        s:[ 0 , 1]       length - end


           b
 for ( end ){

    s
 }
*/
```



####  Dynamic Programming:

```java
class Solution {
    public String longestPalindrome(String s) {
        int n  = s.length();
        boolean[][] dp = new boolean[n][n];
    
        int[] ans = new int[]{0,0};
        // ans stores the start and end indices of the longest palindromic substring found


        // Initializes all single-character substrings as palindromes 
        // because any single character is inherently a palindrome.
        for (int i = 0; i < n; i++){
            dp[i][i] = true;
        }
        //   0  1  2  3  4
        //   b  a  b  a  d
        //b  T, _, _, _, _
        //a  _, T, _, _, _   i+1      (ba)
        //b  _, _, T, _, _
        //a  _, _, _, T, _
        //d  _, _, _, _, T 
        ///     i 

        // dp[i][j] means from i to j whether palindromic


        // This loop checks all consecutive character pairs in the string 
        // and marks them as palindromes in dp 
        // if they are the same. It also updates ans to these indices 
        // if a palindrome is found, considering two-character substrings.
        for (int i = 0; i < n - 1; i++){
            if (s.charAt(i) == s.charAt(i + 1)){
                dp[i][i + 1] = true;
                ans[0] = i;
                ans[1] = i + 1;
            };

        }

        // The outer loop gradually increases the difference between the start and 
        // end indices of the substrings being considered 
        // (diff represents the length of the substring minus one).
        for (int diff = 2; diff < n; diff++){
            // diff = 2; diff < 5; diff++
            for (int i = 0; i < n - diff; i++){
                // The inner loop iterates over all possible 
                // starting indices i for the current length.
                // i = 0; i < 5-2 =3 ; i++  i: [0, 1, 2, 3]   
                int j = i + diff; // end point 0+2 = 2 
                // j is calculated as the end index of the substring.
                if (s.charAt(i) == s.charAt(j) && dp[i + 1][j - 1]){
                    // It checks if the characters at the current start (i) and end (j) are the same
                    // if the substring between them (dp[i + 1][j - 1]) is a palindrome
                    // s.charAt(0) == s.charAt(2)
                    // dp[0+1][2-1] = dp[1][1] 
                    dp[i][j] = true;
                    ans[0] = i;
                    ans[1] = j;
                }
            }
        }

        // diff = 2
        //   0  1  2  3  4
        //   b  a  b  a  d
        //b  T, _, _, _, _
        //a  _, T, _, _, _   i+1      (ba)
        //b  _, _, T, _, _
        //a  _, _, _, T, _
        //d  _, _, _, _, T 
        ///     i 


        int start = ans[0];
        int end = ans[1];
        return s.substring(start, end + 1);
        // Returns the substring from start to end + 1 
        // (as the second argument of substring is exclusive).

    }
}

// TC: O(n^2)
// SC: O(n^2)
```



```java
class Solution {
    public String longestPalindrome(String s) {
        /*
        s: 0 1 2 3 4
        0    T
        1
        2
        3
        4

        [i][j] from i j wether is palindrome
        */


        int n = s.length();

        int[] result = new int[]{0,0};
        boolean[][] memo = new boolean[n + 1][n + 1];
        for (int i = 0; i < n; i++){
            for (int j = i; j < n; j++){
                memo[i][j] = isP(s, i, j);
                if (memo[i][j] == true){
                    int cur = j - i;
                    int subR = result[1] - result[0];
                    if (subR < cur){
                        result[0] = i;
                        result[1] = j;
                    } 
                }
            }
        }

        return s.substring(result[0], result[1] +1);

    }

    private boolean isP(String s, int left, int right){
        while (left < right){
            if (s.charAt(left) != s.charAt(right)){
                return false;
            }
            left++;
            right--;
        }

        return true;
    }
}
```



#### Expand From Centers

```java
class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() < 1){
            return "";
        }

        int start = 0;
        int end = 0;

        for (int i = 0; i < s.length(); i++){
            int len1 = expandFromMiddle(s, i, i);
            int len2 = expandFromMiddle(s, i, i+1);
            int len = Math.max(len1, len2);
            if (len > end - start){
                start = i - ((len -1)/2);
                end = i + (len /2);
            }
            
        }
        
        return s.substring(start, end + 1);
    
    }

    public int expandFromMiddle(String s, int left, int right){
        if (s == null || left > right ){
            return 0;

        }

        while(left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)){
            left--;
            right++;
        }
        return right - left - 1;
    }
}
// TC: O(n^2)

// SC: O(1)
```



```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        int start = 0;
        int end = 0;

        for (int i = 0; i < n; i++){
            for (int j = 0; j <= 1; j++){
                int left = i;
                int right = i + j;
                while(left >= 0 && right <= n - 1 && s.charAt(left) == s.charAt(right)){
                    left--;
                    right++;
                }
                left++;
                right--;

                if (right - left > end - start){
                    start = left;
                    end = right;
                }
            }
        }

        return s.substring(start, end + 1);
    }
}

// TC: O(n^2)
// SC: O(1)
```



Style 1: Handle Odd and Even Palindromes Separately
 Style 2: Merge Them into One



We enumerate
 $i = 0, 1, 2, \dots, 2n - 1$.

- When **$i$ is even**, we use the rule for **odd-length palindromes**:
   initialize
  $$
  l = r = \left\lfloor \frac{i}{2} \right\rfloor
  $$
  For example, when $i = 2$, we have $l = r = 1$.

- When **$i$ is odd**, we use the rule for **even-length palindromes**:
   initialize
  $$
  l = \left\lfloor \frac{i}{2} \right\rfloor,\quad r = \left\lceil \frac{i}{2} \right\rceil
  $$
  For example, when $i = 1$, we have $l = 0$, $r = 1$.

These two cases can be merged into a single unified rule:
$$
l = \left\lfloor \frac{i}{2} \right\rfloor,\quad
r = \left\lceil \frac{i}{2} \right\rceil
= \left\lfloor \frac{i + 1}{2} \right\rfloor
$$
With this formulation, we can enumerate **all odd-length and even-length palindromic substrings exactly once** using a single loop.





#### Manacher's Algorithm

本题需要输出具体的最长回文子串，这需要我们在跑 Manacher 算法的过程中，维护最大的 halfLen[i] 对应的下标 maxI。最后根据下标转换关系，算出 t 中的最长回文子串在 s 中的下标。

```java
class Solution {
    public String longestPalindrome(String s) {
        // Manacher 模板
        // 将 s 改造为 t，这样就不需要讨论 len(s) 的奇偶性，因为新串 t 的每个回文子串都是奇回文串（都有回文中心）
        // s 和 t 的下标转换关系：
        // (si+1)*2 = ti
        // ti/2-1 = si
        // ti 为偶数，对应奇回文串（从 2 开始）
        // ti 为奇数，对应偶回文串（从 3 开始）
        int n = s.length();
        char[] t = new char[n * 2 + 3];
        Arrays.fill(t, '#');
        t[0] = '^';
        for (int i = 0; i < n; i++) {
            t[i * 2 + 2] = s.charAt(i);
        }
        t[n * 2 + 2] = '$';

        // 定义一个奇回文串的回文半径=(长度+1)/2，即保留回文中心，去掉一侧后的剩余字符串的长度
        // halfLen[i] 表示在 t 上的以 t[i] 为回文中心的最长回文子串的回文半径
        // 即 [i-halfLen[i]+1,i+halfLen[i]-1] 是 t 上的一个回文子串
        int[] halfLen = new int[t.length - 2];
        halfLen[1] = 1;

        // maxI 记录最长回文子串在 halfLen 中的下标
        int maxI = 0;
        // boxR 表示当前右边界下标最大的回文子串的右边界下标+1
        // boxM 为该回文子串的中心位置，二者的关系为 r=mid+halfLen[mid]
        int boxM = 0;
        int boxR = 0;
        for (int i = 2; i < halfLen.length; i++) {
            int hl = 1;
            if (i < boxR) {
                // 记 i 关于 boxM 的对称位置 i'=boxM*2-i
                // 若以 i' 为中心的最长回文子串范围超出了以 boxM 为中心的回文串的范围（即 i+halfLen[i'] >= boxR）
                // 则 halfLen[i] 应先初始化为已知的回文半径 boxR-i，然后再继续暴力匹配
                // 否则 halfLen[i] 与 halfLen[i'] 相等
                hl = Math.min(halfLen[boxM * 2 - i], boxR - i);
            }

            // 暴力扩展
            while (t[i - hl] == t[i + hl]) {
                hl++;
                boxM = i;
                boxR = i + hl;
            }

            halfLen[i] = hl;
            if (hl > halfLen[maxI]) {
                maxI = i;
            }
        }

        int hl = halfLen[maxI];
        // 注意 t 上的最长回文子串的最左边和最右边都是 '#'
        // 所以要对应到 s，最长回文子串的下标是从 (maxI-hl)/2 到 (maxI+hl)/2-2
        return s.substring((maxI - hl) / 2, (maxI + hl) / 2 - 1);
    }
}
```



```java
class Solution {
    public String longestPalindrome(String s) {
        StringBuilder sPrime = new StringBuilder("#");
        for (char c: s.toCharArray()) {
            sPrime.append(c).append("#");
        }
        
        int n = sPrime.length();
        int[] palindromeRadii = new int[n];
        int center = 0;
        int radius = 0;
        
        for (int i = 0; i < n; i++) {
            int mirror = 2 * center - i;
            
            if (i < radius) {
                palindromeRadii[i] = Math.min(radius - i, palindromeRadii[mirror]);
            }
            
            while (i + 1 + palindromeRadii[i] < n &&
                   i - 1 - palindromeRadii[i] >= 0 &&
                   sPrime.charAt(i + 1 + palindromeRadii[i]) == sPrime.charAt(i - 1 - palindromeRadii[i])) {
                palindromeRadii[i]++;
            }
            
            if (i + palindromeRadii[i] > radius) {
                center = i;
                radius = i + palindromeRadii[i];
            }
        }
        
        int maxLength = 0;
        int centerIndex = 0;
        for (int i = 0; i < n; i++) {
            if (palindromeRadii[i] > maxLength) {
                maxLength = palindromeRadii[i];
                centerIndex = i;
            }
        }
        
        int startIndex = (centerIndex - maxLength) / 2;
        String longestPalindrome = s.substring(startIndex, startIndex + maxLength);
        
        return longestPalindrome;
    }
}
// TC: O(n)
// SC: O(n)
```

Paper reference: https://dl.acm.org/doi/10.1145/321892.321896



https://www.bilibili.com/video/BV1Sx4y1k7jG/?spm_id_from=333.337.search-card.all.click&vd_source=73e7d2c4251a7c9000b22d21b70f5635
