---
tags:
    - Hash Table
    - Math
    - String
    - Top Interview 150
---

# [13. Roman to Integer](https://leetcode.com/problems/roman-to-integer/)

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, `2` is written as `II` in Roman numeral, just two ones added together. `12` is written as `XII`, which is simply `X + II`. The number `27` is written as `XXVII`, which is `XX + V + II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

- `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
- `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
- `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer.

 

**Example 1:**

```
Input: s = "III"
Output: 3
Explanation: III = 3.
```

**Example 2:**

```
Input: s = "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```

**Example 3:**

```
Input: s = "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

 

**Constraints:**

- `1 <= s.length <= 15`
- `s` contains only the characters `('I', 'V', 'X', 'L', 'C', 'D', 'M')`.
- It is **guaranteed** that `s` is a valid roman numeral in the range `[1, 3999]`.



**Solution:**

本题的难点在于处理六种特殊规则，但这六种特殊规则其实可以统一起来：

设 x=s[i−1], y=s[i]，这是两个相邻的罗马数字。
如果 x 的数值小于 y 的数值，那么 x 的数值要取相反数。例如 IV 中的 I 相当于 −1，CM 中的 C 相当于 −100。
把所有数值相加，即为答案。

代码实现时，可以创建一个哈希表（或者数组），把字符作为 key，对应的数值作为 value，从而避免写一堆 if-else.

```java
class Solution {
    public int romanToInt(String s) {
        Map<Character, Integer> map = new HashMap<>();
        map.put('I', 1);
        map.put('V', 5);
        map.put('X', 10);
        map.put('L', 50);
        map.put('C', 100);
        map.put('D', 500);
        map.put('M', 1000);

        char[] sArray = s.toCharArray();
        int n = s.length();
        int result = 0;

        for (int i = 1; i < n; i++){
            int x = map.get(sArray[i - 1]);
            int y = map.get(sArray[i]);
            if (x < y){
                x = -x;
            }

            result = result + x;
        }

        return result + map.get(sArray[n - 1]);


    }
}
// TC: O(n)
// SC: O(n)
```



```java
class Solution {
    public int romanToInt(String s) {
        Map<Character, Integer> map = new HashMap<>();
        map.put('I', 1);
        map.put('V', 5);
        map.put('X', 10);
        map.put('L', 50);
        map.put('C', 100);
        map.put('D', 500);
        map.put('M', 1000);

        int n  = s.length();

        int result = 0;
        int i = 1;
        char[] sArray = s.toCharArray();
        while(i < n){
            char first = sArray[i - 1];
            char second = sArray[i];
            int firstVal = map.get(first);
            int secondVal = map.get(second);
            
            if (firstVal < secondVal){
                int cur = secondVal - firstVal;
                result = result + cur;
                i = i + 2;
            }else{
                result = result + firstVal;
                i = i + 1;
            }
        }

        if (i == n){
            result = result + map.get(sArray[n-1]);
        }
        return result;
    }
}

// TC: O(n)
// SC: O(n)
```





```java
class Solution {
    public int romanToInt(String s) {
        Map<String, Integer> map = new HashMap<>();
        map.put("I", 1);
        map.put("IV", 4);
        map.put("V", 5);
        map.put("IX", 9);
        map.put("X", 10);
        map.put("XL", 40);
        map.put("L", 50);
        map.put("XC", 90);
        map.put("C", 100);
        map.put("CD", 400);
        map.put("D", 500);
        map.put("CM", 900);
        map.put("M", 1000);

        int result = 0;
        int i = 1;
        int n = s.length();
        //  0123456
        // "MCMXCIV"
        //       i 
        //  
        while(i < n){
            String curTwo = s.substring(i - 1, i + 1); // MC // CM // XC // CI
            // III // I
            //   i 
            if (map.containsKey(curTwo)){
                result = result + map.get(curTwo);// 900
                i = i + 2; // 
            }else{
                result = result + map.get(s.substring(i - 1, i)); // M // C
                // X 1900 + 10 + 100   // 1 
                i = i + 1; // 
            }
        }

        if (i == n){
            result = result + map.get(s.substring(i - 1, i));
        }

        // 1900

        return result;
    }
}

// TC: O(n)
// SC: O(n)
```

