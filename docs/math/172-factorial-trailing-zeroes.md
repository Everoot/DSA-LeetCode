---
tags:
    - Math
    - Top Interviews
---



# [172. Factorial Trailing Zeroes](https://leetcode.com/problems/factorial-trailing-zeroes/)

Given an integer `n`, return *the number of trailing zeroes in* `n!`.

Note that `n! = n * (n - 1) * (n - 2) * ... * 3 * 2 * 1`.

**Example 1:**

```text
Input: n = 3
Output: 0
Explanation: 3! = 6, no trailing zero.
```

**Example 2:**

```text
Input: n = 5
Output: 1
Explanation: 5! = 120, one trailing zero.
```

**Example 3:**

```text
Input: n = 0
Output: 0
```

**Constraints:**

- `0 <= n <= 104`

**Follow up:** Could you write a solution that works in logarithmic time complexity?

**Solution:**

For exmaple $10! = 3628800 = 36288 \times 10^2$ the trailing zeroes is 2.

一般地，\( n! \) 的尾零个数取决于 \( n! \) 可以分解出多少个 10。  由于 \( 10 = 2 \times 5 \)，我们需要知道 \( n! \) 中质因子 2 的个数与质因子 5 的个数, 二者的较小值即为 \( n! \) 的尾零个数。

下面来计算 \( n! \) 的质因子分解中, 质数 \( p \) 的个数.

- 在区间 \([1, n]\) 中：有  $\left\lfloor \frac{n}{p} \right\rfloor$ 个数可以被$ p $整除, 其中又有$\left\lfloor \frac{n}{p^2} \right\rfloor$个数可以被 \( p^2 \) 整除, 所以**恰好能被 \( p \) 整除但不能被 \( p^2 \) 整除**的数有 $\left\lfloor \frac{n}{p} \right\rfloor - \left\lfloor \frac{n}{p^2} \right\rfloor$ 个. 例如在 $[1, 10]$ 中，有 3 个数 \( 2, 6, 10 \) 恰好能被 \( p=2 \) 整除。
- 在 [1, n] 中，有 $\lfloor \frac{n}{p^2} \rfloor$ 个数可以被 $ p^2 $ 整除，  其中又有 $\lfloor \frac{n}{p^3} \rfloor$ 个数可以被 $ p^3 $ 整除，所以恰好能被 $ p^2 $ 整除，但不能被 $ p^3 $ 整除的数有$\lfloor \frac{n}{p^2} \rfloor - \lfloor \frac{n}{p^3} \rfloor$ 个。
- 依此类推，$[1,n]$ 中恰好能被 $ p^{k-1} $ 整除，但不能被 $ p^k $ 整除的数有$\lfloor \frac{n}{p^{k-1}} \rfloor - \lfloor \frac{n}{p^k} \rfloor$ 个。
- 设 $p^k \le n < p^{k+1}$，那么 $[1,n]$ 中恰好能被 $ p^k $ 整除的数有$\lfloor \frac{n}{p^k} \rfloor$ 个。

继续讨论：

- 恰好能被 $p$ 整除的数，有 1 个质因子 $p$，所以这些数一共有 $1 \cdot (\lfloor \frac{n}{p} \rfloor - \lfloor \frac{n}{p^2} \rfloor)$ 个 $p$。
- 恰好能被 $ p^2 $ 整除的数，有 2 个质因子 $ p $，所以这些数一共有 $2 \cdot (\lfloor \frac{n}{p^2} \rfloor - \lfloor \frac{n}{p^3} \rfloor)$ 个 $ p $。
- ……
- 恰好能被 $ p^{k-1} $ 整除的数，有 $k-1$ 个质因子 $ p $，所以这些数一共有 $(k-1) \cdot (\lfloor \frac{n}{p^{k-1}} \rfloor - \lfloor \frac{n}{p^k} \rfloor)$ 个 $ p $。
- 恰好能被 $ p^k $ 整除的数，有 $k$ 个质因子 $ p $, 所以这些数一共有 $k \cdot \lfloor \frac{n}{p^k} \rfloor$ 个 $ p $。

累加，即为 $ n! $ 的质因子分解中质数 $ p $ 的个数：

$$
1 \cdot (\lfloor \frac{n}{p} \rfloor - \lfloor \frac{n}{p^2} \rfloor) + 2 \cdot (\lfloor \frac{n}{p^2} \rfloor - \lfloor \frac{n}{p^3} \rfloor) + \cdots + (k-1) \cdot (\lfloor \frac{n}{p^{k-1}} \rfloor - \lfloor \frac{n}{p^k} \rfloor) + k \cdot \lfloor \frac{n}{p^k} \rfloor
$$
化简得：
$$
\lfloor \frac{n}{p} \rfloor + \lfloor \frac{n}{p^2} \rfloor + \cdots + \lfloor \frac{n}{p^k} \rfloor
$$
由于 $ p $ 越大，上式越小，所以 $ n! $ 中的质因子 5 的个数比 2 少, 我们只需计算上式 $p=5$ 的结果。

此外，有如下恒等式：
$$
\lfloor \frac{n}{p^k} \rfloor = \lfloor \frac{1}{p} \cdot \frac{n}{p^{k-1}} \rfloor = \lfloor \frac{1}{p} \cdot \lfloor \frac{n}{p^{k-1}} \rfloor \rfloor
$$
证明见 [下取整恒等式及其应用](https://zhuanlan.zhihu.com/p/1893240318645732760)。

**下取整恒等式及其应用**

关于 **下取整**，有如下 **恒等式**，其中 \(a\) 为正整数：

\[
\left\lfloor\dfrac{\left\lfloor x \right\rfloor}{a}\right\rfloor = \left\lfloor\dfrac{x}{a}\right\rfloor
\]

例如：

\[
\left\lfloor\dfrac{\left\lfloor 15/2 \right\rfloor}{3}\right\rfloor = \left\lfloor\dfrac{15}{2 \times 3}\right\rfloor = 2
\]

**证明**

设  
\[
m = \left\lfloor\dfrac{\left\lfloor x \right\rfloor}{a}\right\rfloor
\]
即 \(m\) 是 \(\dfrac{\left\lfloor x \right\rfloor}{a}\) 的整数部分，则有：

\[
m \le \dfrac{\left\lfloor x \right\rfloor}{a} < m + 1
\]

即：

\[
am \le \left\lfloor x \right\rfloor < a(m + 1)
\]

由于  
\[
am \le \left\lfloor x \right\rfloor \le x
\]
以及  
\[
x < \left\lfloor x \right\rfloor + 1 \le a(m + 1)
\]

得：

\[
am \le x < a(m + 1)
\]

即：

\[
m \le \dfrac{x}{a} < m + 1
\]

所以 \(m\) 也是 \(\dfrac{x}{a}\) 的整数部分，即：

\[
m = \left\lfloor\dfrac{x}{a}\right\rfloor
\]

故恒等式成立：

\[
\boxed{\left\lfloor\dfrac{\left\lfloor x \right\rfloor}{a}\right\rfloor = \left\lfloor\dfrac{x}{a}\right\rfloor}
\]

**应用 1**

[1553. Minimum Number of Days to Eat N Oranges](https://leetcode.com/problems/minimum-number-of-days-to-eat-n-oranges/)

```python
class Solution {
    private final Map<Integer, Integer> memo = new HashMap<>();

    public int minDays(int n) {
        if (n <= 1) {
            return n;
        }
        Integer res = memo.get(n);
        if (res != null) { // 之前计算过
            return res;
        }
        res = Math.min(minDays(n / 2) + n % 2, minDays(n / 3) + n % 3) + 1;
        memo.put(n, res); // 记忆化
        return res;
    }
}
```

该代码在递归过程中，会遇到多少个不同的 `i`？

由恒等式可知, i和n有如下关系

\[
i = \left\lfloor\dfrac{n}{2^x 3^y}\right\rfloor
\]

其中x和y均为整数且：

\[
0 \le x \le \left\lfloor \log_2 n \right\rfloor + 1, \quad 0 \le y \le \left\lfloor \log_3 n \right\rfloor + 1
\]

所以共有：

\[
\mathcal{O}(\log^2 n)
\]

个不同的状态。

**应用 2**

**[LeetCode 3296. 移山所需的最少秒数](https://leetcode.cn/problems/minimum-number-of-seconds-to-make-mountain-height-zero/description/)**

特别地，把 \(x\) 替换成 \(\sqrt{x}\)，得：

\[
\left\lfloor\dfrac{\sqrt{x}}{a}\right\rfloor = \left\lfloor\dfrac{\lfloor \sqrt{x}\rfloor}{a}\right\rfloor
\]

该式在计算 **一元二次不等式** 的最大/最小整数解时有用。  Python3 可以用 `math.isqrt` 代替 `** 0.5`。

**练习1**

证明如下恒等式，其中 \(a\) 为正整数：

\[
\left\lceil\dfrac{\left\lceil x \right\rceil}{a}\right\rceil = \left\lceil\dfrac{x}{a}\right\rceil
\]

**练习 2**

把一个正整数 \(n\) 不断地除以 2，最少需要操作多少次，才能把 \(n\) 变成 1？

**解答**

设经过 \(k\) 次操作后变成 1：

\[
\left\lceil\dfrac{n}{2^k}\right\rceil = 1
\]

由于：

\[
\dfrac{n}{2^k} \le \left\lceil\dfrac{n}{2^k}\right\rceil = 1
\]

所以：

\[
2^k \ge n
\]

即：

\[
k \ge \log_2 n
\]

取整数：

\[
k = \left\lceil \log_2 n \right\rceil
\]

即为答案。

---

**注：** 编程计算时，  
\[
\left\lceil \log_2 n \right\rceil
\]
等价于 \((n - 1)\) 的二进制长度（规定 0 的二进制长度为 0）。

注意该恒等式是一个递推式，我们可以用 $\lfloor \frac{n}{p} \rfloor$ 算出 $\lfloor \frac{n}{p^2} \rfloor$, 再用 $\lfloor \frac{n}{p^2} \rfloor$ 算出 $\lfloor \frac{n}{p^3} \rfloor$，依此类推。

**Solution:**

```java
class Solution {
    public int trailingZeroes(int n) {
       int ans = 0;
       while(n > 0){
         // 循环 k 次后，n 变成了 floor(n/5^k)
         n = n/5;
         ans = ans + n;
       } 

       return ans;
    }
}

// TC: O(logn)
// SC: O(1)
```
