---
tags:
    - Two Pointers
    - String
    - String Matching
    - Knuth-Morris-Pratt
    - Top Interviews
---

# [28. Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)

Given two strings `needle` and `haystack`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.

 

**Example 1:**

```
Input: haystack = "sadbutsad", needle = "sad"
Output: 0
Explanation: "sad" occurs at index 0 and 6.
The first occurrence is at index 0, so we return 0.
```

**Example 2:**

```
Input: haystack = "leetcode", needle = "leeto"
Output: -1
Explanation: "leeto" did not occur in "leetcode", so we return -1.
```



Solution:

Slidewindow:

```java
class Solution {
    public int strStr(String haystack, String needle) {
        int n = haystack.length();
        int m = needle.length();

        for (int i = 0; i <= n - m; i++){
            for (int j = 0; j < m; j++){
                if (haystack.charAt(i + j) != needle.charAt(j)){
                    break;
                } 

                if (j == m - 1){
                    return i;
                }
            }
        }

        return -1;
    }
}

// TC: O(n*m)
// SC: O(1)
```

```java
class Solution {
    public int strStr(String haystack, String needle) {
        int n = haystack.length();
        int m = needle.length();
        if (n < m){
            return -1;
        }
        int result = 0;
        int i = 0;
        // 012345678
        // sadbutsad
        // 012345678910
        // mississippi
        //      i 
        // issip
        // j 
        while(i < n){
            // i < 10
            char curChar = haystack.charAt(i);
            // m
            int rem = i; // 0
            if (curChar == needle.charAt(0)){
                // s

                int j = 0;
                while(i < n && j < m){// j < 3
                    if (haystack.charAt(i) == needle.charAt(j)){
                        // s == s
                        i++; // i++ 1   2  3
                        j++; // j++ 1   2  3
                    }else{
                        break;
                    }
                }

                if (j == m){ // j == 3
                    return rem;
                }
                i = rem + 1;
            }else{
                i++;
            }
    
        }

        return -1;
    }
}
```



```java
class Solution {
    public int strStr(String haystack, String needle) {
       if (haystack == null || needle == null || haystack.length() < needle.length()){
           return -1;
       } 

       if (needle.length() ==  0){
           return 0;
       }

       for (int i = 0; i <= haystack.length() - needle.length(); i++){
           int j = 0;
           while(j < needle.length() && haystack.charAt(j+i) == needle.charAt(j)){
               j++;
           }

           if (j == needle.length()){
               return i;
           }
       }   
       return -1;
    }

}

/*
    l   e   e   t   c   o   d   e
    i

    l  e    e   t  o
    j

*/
```



```java
class Solution {
    public int strStr(String haystack, String needle) {
        int n = haystack.length();
        int m = needle.length();
        if(n < m) {return -1;}
        for(int i = 0; i <= n-m; i++) {
          int j;
          for(j = 0; j < m; j++) {
            if(haystack.charAt(i+j) != needle.charAt(j)) {
              break;
            }
          }
          if(j == m) {
            return i;
          }
        }
      return -1;
    }
}
```



```java
class Solution {

    // CONSTANTS
    final int RADIX_1 = 26;
    final int MOD_1 = 1000000033;
    final int RADIX_2 = 27;
    final int MOD_2 = 2147483647;

    // Return Array of Hash Values
    public long[] hashPair(String string, int m) {
        long hash1 = 0, hash2 = 0;
        long factor1 = 1, factor2 = 1;

        for (int i = m - 1; i >= 0; i--) {
            hash1 += ((int) (string.charAt(i) - 'a') * (factor1)) % MOD_1;
            factor1 = (factor1 * RADIX_1) % MOD_1;
            hash2 += ((int) (string.charAt(i) - 'a') * (factor2)) % MOD_2;
            factor2 = (factor2 * RADIX_2) % MOD_2;
        }

        return new long[] { hash1 % MOD_1, hash2 % MOD_2 };
    }

    public int strStr(String haystack, String needle) {
        int m = needle.length();
        int n = haystack.length();
        if (n < m)
            return -1;

        // COMPUTING CONSTANTS
        long MAX_WEIGHT_1 = 1;
        long MAX_WEIGHT_2 = 1;
        for (int i = 0; i < m; i++) {
            MAX_WEIGHT_1 = (MAX_WEIGHT_1 * RADIX_1) % MOD_1;
            MAX_WEIGHT_2 = (MAX_WEIGHT_2 * RADIX_2) % MOD_2;
        }

        // Compute hash pair of needle
        long[] hashNeedle = hashPair(needle, m);
        long[] hashHay = { 0, 0 };

        // Check for each m-substring of haystack, starting at index windowStart
        for (int windowStart = 0; windowStart <= n - m; windowStart++) {
            if (windowStart == 0) {
                // Compute hashPair of First Substring
                hashHay = hashPair(haystack, m);
            } else {
                // Update Hash Pair using Previous Hash Value in O(1)
                hashHay[0] = ((hashHay[0] * RADIX_1) % MOD_1
                        - ((int) (haystack.charAt(windowStart - 1) - 'a') * MAX_WEIGHT_1) % MOD_1
                        + (int) (haystack.charAt(windowStart + m - 1) - 'a') + MOD_1) % MOD_1;
                hashHay[1] = ((hashHay[1] * RADIX_2) % MOD_2
                        - ((int) (haystack.charAt(windowStart - 1) - 'a') * MAX_WEIGHT_2) % MOD_2
                        + (int) (haystack.charAt(windowStart + m - 1) - 'a') + MOD_2) % MOD_2;
            }

            // If the hash matches, return immediately. Probability of Spurious Hit tends to zero.
            if (hashNeedle[0] == hashHay[0] && hashNeedle[1] == hashHay[1]) {
                return windowStart;
            }
        }

        return -1;
    }
}
```



P.S.    1, 2 在string I 里

Q3(Strstr). Substring problem: how to determine whether a string is a substring of another string.

java API index of (string str) https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)

return the index of the first occurrence of the specified substring, or -1 if there is no such occurrence.

Example:

text (s1) = "a	b	**c	d	e**";	length = m

pattern (s2) = "c	d	e";	length = n

return 2



```java
class Solution {
    public int strStr(String haystack, String needle) {
        int trackIndex = 0;
        int result = 0;
        int i = 0;
        int n = haystack.length();
        int j = 0;
        int m = needle.length();
        while (i < n){
            char cur = haystack.charAt(i);
            int rem = i;
            j = 0;
            if (cur == needle.charAt(0)){
                while(i < n && j < m && haystack.charAt(i) == needle.charAt(j)){
                    j++;
                    i++;
                }
                if (j == m){
                    return rem;
                }else{
                    i = rem + 1;
                }

            }else{
                i++;
            }
        }
        return -1;
    }
}

// TC: O(n×m)
// SC: O(1)
```



### **Method 1: Brute Force O($n^2$)**

for every starting inde in s1:

​		[0  ...  s1.length() - s2.length()]

​		try to match every chars in s1 compare to s2

```java
package TheoreticalClass.Class9StringII;

public class SubstringFinding {
    public static void main(String[] args){
        String string1 = "abcde";
        String string2 = "cde";
        System.out.println("BruteForce:" + helper(string1, string2));
        System.out.println("BruteForce2:" + helper2(string1, string2));
    }

    public static int helper(String s1, String s2){
        if (s1 == null || s2  == null || s1.length() < s2.length()){
            return -1;
        }
        if (s2.length() == 0){
            return 0;
        }
        int result = 0;
        int count = 0;
        int index = 0;

        // a    b      c    d   e
        //                  i
        // c    d     e
        //     j
        for(int i = 0; i < s1.length(); i++){
            for (int j = 0; j < s2.length(); j++){
                if (s1.charAt(i) == s2.charAt(j)) {
                    if (j == 0 || i == 0){
                        count = 1;
                    }else if (s1.charAt(i-1) == s2.charAt(j-1)){
                        count++;
                    }else{
                        count = 1;
                    }
                }
                if (count >= result){
                    index = i - count + 1;
                }else {
                    index = i - result + 1;
                }
                result = Math.max(count, result);

            }
        }
        return index;
    }

    // class 参考答案
    public static int helper2(String s1, String s2){
        if (s1 == null || s2 == null || s1.length() < s2.length()){
            return -1;
        }
        if (s2.length() == 0){ // 容易漏掉    空串是任何空串的substring
            return 0;
        }
        // i： every s1 starting index
        for (int i = 0; i <= s1.length() - s2.length(); i++){  // 等号不能去掉
            int j = 0;
          //保持i的站位不动, j向后延伸
            while (j < s2.length() && s1.charAt(i + j) == s2.charAt(j)){
                j++;
            }
            if (j == s2.length()){
                return i;
            }
        }
        return -1;
    }
    /*
    s1 = a  b   c   d   e
         i  i+1 i+2 i+3 i+4
    s2 = cde
           j
     */
}
```

```
BruteForce:2
BruteForce2:2

Process finished with exit code 0
```

所有指针的题都不要偷懒, 把它的意义写下来

i: every s1 starting index

问这个等于号能不能去掉

别慌 过例子

s1 = a

​		i 

s2 = a

0要进去比一下, 所以等号不能去掉

TC: O(m*n)

SC: O(1)



High Level: for all possible starting index i from 0 to s1.length -2s.length

​					check from i pattern.length char == pattern

​								check i 后面的每一位是否和pattern里的每一位相同



### Method 2 : Rabin-Karp (P2)

http://en.wikipedia.org/wiki/Rabin%E2%80%93Karp_algorithm

s2 = "c	d	e"		hash("cde") = x0

s1 = "a	b	c	d	e";

​          \______________________

​		  a	==b	c==	= X1

​				==b	c==	d	=	X2

​					   c	d	e	= X0

**principle:** If we can hash the short string to a unique integer (via a hash function without collision), then we can just compare each substring of s1's hashed value and compare it with s2's hash value.

Assumption: only lower case letter (base = 26 a--z)

a = 97 (ASCII) -> 0

b = 98 -> 1

...

z 		 -> 25

=>

'a' - 'a' = 0

'b' - 'a' = 1

...

'z' - 'a' = 25

x - 'a' = ?



类似于10进制计算:

123 = 1 * 10 ^ 2 + 2 * 10 ^ 1 + 3 * 10 ^ 0

125 = (10 进制) =  1 * 10 ^ 2 				+					2 * 10 ^ 1				5*10^0

​								100							+					20					+ 		5

abc = 012 (26进制) = 0 * 26 ^ 2 + 1 * 26 ^1 + 2 * 26 ^ 0 = 28

bcd = 123 (26进制) = 1 * 26 ^ 2 + 2 * 26 ^1 + 3 * 26^0 = 731

cde = 234 (26进制) = 2 * 26 ^2 + 3 * 26 ^ 1 + 4 * 26 ^0 = 1434

a				     b				c

0*26^2 + 1 * 26 ^1 + 2 * 26 ^ 0			          											= 28

​						b				c					d

​				 1 * 26 ^2	+ 2 * 26^1 + 3 * 26 ^0 			                              = 731 



1 2 3 -> 2 3 4

​              234    =    (123 - 1*10 ^2 ) * 10  + 4 = 234              O(1) 

​										  23

前面踢一个后面加一个O(1)



a	b	c

​	  b	c	d

bcd = (28(abc) - 0 * 26 ^2) * 26 + 3 



hash("bcd") = (hash("abc") - ('a' - 'a') * 26 ^2) + ('d' - 'a') * 26 ^ 0

​						c							d						e

​				2 * 26 ^2	+		3* 26^1	+	4 * 26 ^0  								= 1434

Initialization: compute hash(s2) -- O(n) 									n = s2.length()

Each time we move the sliding window 		m = s1.length()

Time complexity for each step O(1)

Because we have m-n sliding windows, the total time complexity is O(m-n)

1. remove the leftmost item from the polynomial function
2. all the rest items of (bc's hashed value) x 26 
3. add new item d



starting index has range of 

0 --> s1.size() - s2.size()

In total we have m - n +1 windos. --> O(m) time in average

Total time = O(m + n) average, O(mn) worst case



我用了这招, 我能省下哪儿段时间呢?

就是你从一个窗口比较下一个窗口的时候, 其实你是可以通过上一个窗口hash值 O(1)的时间知道下一值, 并且对比S2也是O(1)的, 所以可以降低到O(m+n)

```java
		public static int strStr(String haystack, String needle) {
        // We need a prime to minimize the chance of collisions, as in a HashMap.
        // The reason a prime is better is that when we generate hashcodes that are
        // multiples of each other, they don't land on the same bucket when we calculate
        // their mod (num_buckets) (unless they are multiples of the chosen prime).
        // As of where I got this from, I just searched for primes on Wikipedia ;)
        final int prime = 9999991;
        // Most people use a base here representative of the base the data is encoded in.
        // I have found 10 to be more intuitive and to work as fast as others,
        // at least for the leetcode test dataset.
        // Sedgewick uses 256, so feel free to switch to that, or to 26 if you restrict your values
        // to a-z.
        final int base = 10;

        // Handle cornercase: needle is empty
        if (needle.length() == 0) return 0;
        // Handle cornercase: needle is longer than haystack
        if (haystack.length() < needle.length()) return -1;

        // Since we are expressing the string in the hashcode as
        // char[i]*base^N + char[i-1] * base^N-1....+char[window.length()-1],
        // when we remove the most significant digit in each step a few lines down from here,
        // we need to multiply the letter we are removing from the window by the base elevated
        // to the number of digits in the window (char[i]*base^N) mod prime.
        // Needle (and the window) can have up to 50,000 characters as per requirements,
        // so this can go up to Math.pow(10, 50,000), which in Java just returns "Infinity" - it's a huge number.
        // To mitigate this, since we are going to be using this number to calculate a hash mod prime,
        // we can multiply the base n times and calculate its mod prime factor by factor.
        // This works because (a*b)%p = ((a%p)*(b%p))%p
        // (https://en.wikipedia.org/wiki/Modulo_operation#Properties_(identities))
        // Here we pre-calculate this number so we can reuse it, as we must do it iteratively
        // and it would consume cycles character by character otherwise.
        int maxBase = 1;
        for (int i = 1; i < needle.length(); i++) {
            maxBase = (maxBase * base) % prime;
        }

        int needlehash = hash(needle, needle.length(), base, prime);
        int haystackhash = hash(haystack, needle.length(), base, prime);

        // Did we get lucky with the first (needle.length()) characters?
        if (needlehash == haystackhash) return 0;

        // This loop slides the window of size needle.length() along the haystack.
        // For example for [a,b,c,d,...] and a needle of size 3,
        // it goes from [a,b,c] to [b,c,d] and so on.
        // At each step, it recalculates the window's hashcode in O(1), by reusing the previous result. This is the key of the algorithm.
        // For more info, please check https://en.wikipedia.org/wiki/Rolling_hash
        for (int i = needle.length(); i < haystack.length(); i++) {
            // Take first char out of the hash. You may note here that instead of subtracting the value for the most significant
            // character directly from haystackhash, I'm subtracting it from haystackhash+prime. The reason is that, since I'm
            // subtracting 2 numbers mod prime, the result could easily become negative. To prevent this, we add prime as the result
            // should be the same (a%p = (a+p)%p) https://en.wikipedia.org/wiki/Modulo_operation#Properties_(identities)
            // since now the number on the left of the subtraction is larger than the number on the right, the result
            // cannot be negative.
            haystackhash = (haystackhash+prime - (maxBase*haystack.charAt(i-needle.length())%prime))%prime;

            // Shift to the left, add new char
            haystackhash = (haystackhash*base + haystack.charAt(i))%prime;

            // hashes matching can mean a match or a collision. Only way to tell one from the other
            // is to linearly verify if the match on hash is also a match character by character.
            if (needlehash == haystackhash && checkSolution(haystack, needle, i-needle.length()+1)) return i-needle.length()+1;
        }

        return -1;
    }

    // Calculates a hashcode similar to how java.lang.String does it.
    // We reimplement it here because we need control over what the prime and the base is.
    // Java uses base=31 and prime=Integer.MAX_VALUE (implicitly)
    private static int hash(String chars, int count, int base, int prime) {
        int hash = 0;
        for (int i = 0; i < count; i++) {
            char c = chars.charAt(i);
            hash = (hash * base + c) % prime;
        }

        return hash;
    }

    // Check solution linearly, fail fast on first character not matching
    private static boolean checkSolution(String haystack, String needle, int i) {
        // Many strings have the same hashcode, we need to check whether this is a collision
        for (int j = 0; j < needle.length(); j++) {
            if (haystack.charAt(j+i) != needle.charAt(j)) return false;
        }

        return true;
    }
```







KMP

