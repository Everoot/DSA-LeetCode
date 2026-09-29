---
tags:
    - Math
    - Top Interviews
    - Microsoft
---

# [9. Palindrome Number](https://leetcode.com/problems/palindrome-number/)

Given an integer `x`, return `true` *if* `x` *is a* ***palindrome****, and* `false` *otherwise*.

 

**Example 1:**

```
Input: x = 121
Output: true
Explanation: 121 reads as 121 from left to right and from right to left.
```

**Example 2:**

```
Input: x = -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```

**Example 3:**

```
Input: x = 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```

 

**Constraints:**

- `-231 <= x <= 231 - 1`

 

**Follow up:** Could you solve it without converting the integer to a string?

 **Solution:**

**前置知识一：如何遍历一个整数**

如果不把 *x* 转成字符串，要怎么做？

我们可以不断地取 *x* 的最低位（模 10），去掉 *x* 的最低位（除以 10），直到 *x*=0。

例如 x=123：

通过 $x \mod10$ 取到个位数 3，然后把 x 除以 10（下取整），得到 x=12。
再次 $x \mod10$ 取到十位数 2，然后把 x 除以 10（下取整），得到 x=1。
最后 $x \mod10$ 取到百位数 1，然后把 x 除以 10（下取整），得到 x=0。此时完成遍历，退出循环。

**前置知识二：如何反转一个整数**
比如我们现在有一个数 56，如何把 7 加到 56 的末尾？

把 56 乘以 10，再加上 7，就得到了 567。

一般地，如果要把数字 b 加到整数 a 的末尾，我们可以计算 $a⋅10+b$。

如果要把 x=123 反转，我们可以按照前置知识一中的方法，从低到高遍历 x 的每一位，即 3,2,1。

初始化 rev=0，依次把 3,2,1 加到 rev 的末尾，即：

更新 rev 为 $rev⋅10+3=3$。
更新 rev 为 $rev⋅10+2=32$。
更新 rev 为 $rev⋅10+1=321$。
最终得到了 x 反转后的结果 rev=321。



**本题思路**
暴力想法是，根据前置知识二，把 x 整体反转，得到 rev，然后判断 rev 和 x 是否相等。

但实际上，对于回文数来说，只需判断 x 的左半部分是否等于右半部分就行。

**Example 1:**

比如 x=1221，计算过程如下：

1. 初始化 rev=0。
2. 计算 xmod10=1，把 1 加到 rev 的末尾，然后把 x 除以 10（下取整），现在 rev=1，x=122。
3. 计算 xmod10=2，把 2 加到 rev 的末尾，然后把 x 除以 10（下取整），现在 rev=12，x=12。
4. rev=x，说明 x 是（长度为偶数的）回文数。

**Example 2:**

比如 x=121，计算过程如下：

1. 初始化 rev=0。
2. 计算 xmod10=1，把 1 加到 rev 的末尾，然后把 x 除以 10（下取整），现在 rev=1，x=12。
3. $rev = \left \lfloor \frac{x}{10} \right \rfloor $, 说明 *x* 是（长度为奇数的）回文数。



**算法**

1. 初始化 rev=0。
2. 如果$rev<\left \lfloor \frac{x}{10} \right \rfloor$, 循环执行第三步。
3. 每次循环，更新 rev 为 rev⋅10+xmod10，然后把 x 除以 10（下取整）。
4. 循环结束后，如果 rev=x，说明 x 是（长度为偶数的）回文数；如果$rev = \left \lfloor \frac{x}{10} \right \rfloor $ , 说明 x 是（长度为奇数的）回文数。其余情况，x 不是回文数。

注：当 x 是奇数长度回文数时，这个做法比官方题解少循环一次。



**细节**

特判 x<0 的情况，此时 x 一定不是回文数。

特判 x>0 且 x 个位数是 0 的情况，由于 x 的最高位一定不是 0，所以 x 一定不是回文数。

```java
class Solution {
    public boolean isPalindrome(int x) {
       if (x < 0 || x > 0 && x % 10 == 0){
        return false;
       } 
       int rev = 0;
       while(rev < x / 10){
        rev = rev * 10 + x % 10;
        x = x / 10;
       }

       return rev == x || rev == x /10;
    }
}

// TC: O(logx), if x<=0 then O(1)
// SC: O(1)
```

