---
tags:
    - Hash Table
    - Math
    - String
---

# [12. Integer to Roman](https://leetcode.com/problems/integer-to-roman/)

Seven different symbols represent Roman numerals with the following values:

| Symbol | Value |
| ------ | ----- |
| I      | 1     |
| V      | 5     |
| X      | 10    |
| L      | 50    |
| C      | 100   |
| D      | 500   |
| M      | 1000  |

Roman numerals are formed by appending the conversions of decimal place values from highest to lowest. Converting a decimal place value into a Roman numeral has the following rules:

- If the value does not start with 4 or 9, select the symbol of the maximal value that can be subtracted from the input, append that symbol to the result, subtract its value, and convert the remainder to a Roman numeral.
- If the value starts with 4 or 9 use the **subtractive form** representing one symbol subtracted from the following symbol, for example, 4 is 1 (`I`) less than 5 (`V`): `IV` and 9 is 1 (`I`) less than 10 (`X`): `IX`. Only the following subtractive forms are used: 4 (`IV`), 9 (`IX`), 40 (`XL`), 90 (`XC`), 400 (`CD`) and 900 (`CM`).
- Only powers of 10 (`I`, `X`, `C`, `M`) can be appended consecutively at most 3 times to represent multiples of 10. You cannot append 5 (`V`), 50 (`L`), or 500 (`D`) multiple times. If you need to append a symbol 4 times use the **subtractive form**.

Given an integer, convert it to a Roman numeral.

 

**Example 1:**

**Input:** num = 3749

**Output:** "MMMDCCXLIX"

**Explanation:**

```
3000 = MMM as 1000 (M) + 1000 (M) + 1000 (M)
 700 = DCC as 500 (D) + 100 (C) + 100 (C)
  40 = XL as 10 (X) less of 50 (L)
   9 = IX as 1 (I) less of 10 (X)
Note: 49 is not 1 (I) less of 50 (L) because the conversion is based on decimal places
```

**Example 2:**

**Input:** num = 58

**Output:** "LVIII"

**Explanation:**

```
50 = L
 8 = VIII
```

**Example 3:**

**Input:** num = 1994

**Output:** "MCMXCIV"

**Explanation:**

```
1000 = M
 900 = CM
  90 = XC
   4 = IV
```

 

**Constraints:**

- `1 <= num <= 3999`



**Solution:**

把 num 拆分成千位数、百位数、十位数和个位数，分别用罗马数字表示。

例如示例 1 的 num=3749，拆分后对应的罗马数字分别为 MMM,DCC,XL,IX。

根据题意，从千位数到个位数，阿拉伯数字与罗马数字的转换关系为：

千位从 1 到 3 依次为 M,MM,MMM。
百位从 1 到 9 依次为 C,CC,CCC,CD,D,DC,DCC,DCCC,CM。
十位从 1 到 9 依次为 X,XX,XXX,XL,L,LX,LXX,LXXX,XC。
个位从 1 到 9 依次为 I,II,III,IV,V,VI,VII,VIII,IX。
把 num 拆分成各个数位的公式为：

千位：$⌊\frac{1000}{num}⌋$。例如 $\lfloor \frac{1000}{3749} \rfloor$

百位：$\lfloor \frac{100}{num} \rfloor$ 例如 $\lfloor \frac{3749}{100} \rfloor \mod 10 = 37 \mod 10 = 7$

十位：$\lfloor \frac{num}{10} \rfloor \mod 10$ 例如 $\lfloor \frac{3749}{10} \rfloor \mod 10 = 4 $ 

个位：$num \mod 10$。例如 $3749 \mod 10 = 9$。
用字符串数组存储罗马数字，用上述公式计算出的数位当作数组下标，取出对应的罗马数字（字符串）。把这些字符串拼接起来，得到答案。



```java
class Solution {
    private static final String[][] R = new String[][]{
        {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"},
        {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"},
        {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"},
        {"", "M", "MM", "MMM"}
    };
    public String intToRoman(int num) {
        return R[3][num/1000] + R[2][num / 100 % 10] + R[1][num / 10 % 10] + R[0][num % 10];

    }
}

// TC: O(n)
// SC: O(1)
```



I = 1

II = 2

III = 3

IV = 4 (-1 + 5)

V = 5

VI = 6

VII = 7

VIII = 8

IX =  9 (- 1 + 10)

X = 10

...

L = 50

C = 100

D = 500

M = 1000



```java
class Solution {
    public String intToRoman(int num) {
        Map<Integer, String> map = new HashMap<>();
        map.put(1, "I");
        map.put(4, "IV");
        map.put(5, "V");
        map.put(9, "IX");
        map.put(10, "X");
        map.put(40, "XL"); // (-10 + 50)
        map.put(50, "L");
        map.put(90, "XC"); 
        map.put(100, "C");
        map.put(400, "CD");
        map.put(500, "D");
        map.put(900, "CM");
        map.put(1000, "M");
    
        int[] values = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};

        StringBuilder sb = new StringBuilder();

        for (int v : values){
            while(num >= v){
                sb.append(map.get(v));
                num = num - v;
            }
        }
        return sb.toString();
    }

}

/*
Symbol	Value
I	1
V	5
X	10
L	50
C	100
D	500
M	1000

num = 3749
M        M.    M      D     C     C.    X   L    I.   X
1000 + 1000 + 1000 + 500 + 100 + 100 + 10 + 50 + 1 + 10

s

        f 
if ( f > s)  - > cur.   +  (f - 
*/

// TC: O(n)
// SC: O(1)
```



```java
class Solution {
    public String intToRoman(int num) {
        Map<Integer, String> map = new HashMap<>();
        map.put(1, "I");
        map.put(4, "IV");
        map.put(5, "V");
        map.put(9, "IX");
        map.put(10, "X");
        map.put(40, "XL"); // (-10 + 50)
        map.put(50, "L");
        map.put(90, "XC"); 
        map.put(100, "C");
        map.put(400, "CD");
        map.put(500, "D");
        map.put(900, "CM");
        map.put(1000, "M");

        StringBuilder sb = new StringBuilder();
        int time = 1000;

      //  3749 
        while(time > 0){
            int curD = num;
            num = num;
            if (time != 0){
                curD = num / time; // 3749 /1000 = 3 
                num = num % time; // 749
            }

            if (curD == 0){
                time = time / 10;
                continue;

            }
						
          /*
          If the value does not start with 4 or 9, select the symbol of the maximal value that can be subtracted from the input, append that symbol to the result, subtract its value, and convert the remainder to a Roman numeral.
          */
            if (curD != 4 && curD != 9){
                if (curD == 5){
                    sb.append(map.get(5 * time));
                }

                if (curD < 4){
                    for (int i = 0; i < curD; i++){
                        sb.append(map.get(time));// 3 
                        // 1000 
                    }
                }

                if (curD > 5){
                    sb.append(map.get(5 * time));
                    for (int i = 0; i < curD - 5; i++){
                        sb.append(map.get(time));
                    }
                }
                time = time / 10;

            }

            if (curD == 4){
                sb.append(map.get(4 * time));
                time = time / 10;
                continue;
            }

            if (curD == 9){
                sb.append(map.get(9 * time));
                time = time / 10;
                continue;
            }

        }
        
        return sb.toString();
    }


}

/*
Symbol	Value
I	1
V	5
X	10
L	50
C	100
D	500
M	1000

num = 3749
M        M.    M      D     C     C.    X   L    I.   X
1000 + 1000 + 1000 + 500 + 100 + 100 + 10 + 50 + 1 + 10

s

        f 
if ( f > s)  - > cur.   +  (f - 
*/
```



```java
class Solution {
    public String intToRoman(int num) {
        Map<Integer, String> map = new HashMap<>();
        map.put(1, "I");
        map.put(4, "IV");
        map.put(5, "V");
        map.put(9, "IX");
        map.put(10, "X");
        map.put(40, "XL"); // (-10 + 50)
        map.put(50, "L");
        map.put(90, "XC"); 
        map.put(100, "C");
        map.put(400, "CD");
        map.put(500, "D");
        map.put(900, "CM");
        map.put(1000, "M");

        StringBuilder sb = new StringBuilder();
        int time = 1000;

      //  3749 
        while(time > 0){
            int curD = num / time;
            num = num % time;

            if (curD == 0){
                time = time / 10;
                continue;

            }

            if (curD == 4){
                sb.append(map.get(4 * time));
                time = time / 10;
                continue;
            }else if (curD == 9){
                sb.append(map.get(9 * time));
                time = time / 10;
                continue;
            }else{
                if (curD >= 5){
                    sb.append(map.get(5 * time));
                    curD = curD - 5;
                }
                for (int i = 0; i < curD; i++){
                    sb.append(map.get(time));
                }
                time = time / 10;

            }

        }
        
        return sb.toString();
    }


}

// TC: O(n)
// SC: O(1)
```

