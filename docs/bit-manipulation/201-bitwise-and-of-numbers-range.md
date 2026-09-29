---
tags:
    - Bit Manipulation
    - Top Interviews
---

# [201. Bitwise AND of Numbers Range](https://leetcode.com/problems/bitwise-and-of-numbers-range/)

Given two integers `left` and `right` that represent the range `[left, right]`, return *the bitwise AND of all numbers in this range, inclusive*.

**Example 1:**

```
Input: left = 5, right = 7
Output: 4
```

**Example 2:**

```
Input: left = 0, right = 0
Output: 0
```

**Example 3:**

```
Input: left = 1, right = 2147483647
Output: 0
```

**Constraints:**

- `0 <= left <= right <= 231 - 1`

**Solution:**

```java
class Solution {
    public int rangeBitwiseAnd(int left, int right) {
        int cnt = 0;
 				// 找到left和right的公共前缀，并统计这个过程右移了多少次
        while(left != right){
            cnt++;
            left = left >> 1;
            right = right >> 1;
        }
				// 将这个公共前缀还原回去
        return left << cnt;
    }
}

// TC: O(n)
// SC: O(1)
```



**举例**
$$
left &= 110010 \\
right &= 110110
$$
在[left, right]中的二进制数为
$$
110010 \\
110011 \\
110100 \\
110101 \\
110110 \\
$$
这些二进制数的公共前缀都是110, 对于后三位, 由于包含了
$$
110011 \\
110100 \\
$$
这两个数，所以后三位的 AND 一定是 0。

所以答案是1110000.

一般地, 把left(或者right)的后三位置为0, 就可以得到最终答案了.



**怎么求出需要置0的长度?**

注意$110\underline{0}10$ 和 $110\underline{1}10$画下划线的地方, 这是它们俩从左往右看, 首次不同的位置.

这可以通过计算left和right的XOR得到, 即100.

计算100的长度(3), 就是我们要置为0的长度了.



**怎么用位运算置0?**

设left 和 right的XOR的长度为m.

根据 [从集合论到位运算](https://leetcode.cn/circle/discuss/CaOJ45/)，我们相当于去掉集合 *left* 中的元素 0,1,2,…,*m*−1。

元素0,1,2,..., m -1 组成的集合为$2^m -1$.

所以答案为
$$
left \&~(2^m -1)
$$
写成代码就是:

```java
class Solution {
    public int rangeBitwiseAnd(int left, int right) {
        int m = 32 - Integer.numberOfLeadingZeros(left ^ right);
        return left & ~ ((1 << m) - 1);
    }
}

// TC: O(1)
// SC: O(1)
```

> 这解法.. 不适合我 呜呜呜
>
> ????