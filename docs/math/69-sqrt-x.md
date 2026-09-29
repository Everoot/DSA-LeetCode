---
tags:
    - Math
    - Binary Search
    - Top Interviews
---

# [69. Sqrt(x)](https://leetcode.com/problems/sqrtx/)

Given a non-negative integer `x`, return *the square root of* `x` *rounded down to the nearest integer*. The returned integer should be **non-negative** as well.

You **must not use** any built-in exponent function or operator.

- For example, do not use `pow(x, 0.5)` in c++ or `x ** 0.5` in python.

 

**Example 1:**

```
Input: x = 4
Output: 2
Explanation: The square root of 4 is 2, so we return 2.
```

**Example 2:**

```
Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since we round it down to the nearest integer, 2 is returned.
```

 

**Constraints:**

- `0 <= x <= 231 - 1`

**Solution:**

**题意:**

输入非负整数 *x*，返回最大的非负整数 *m*，满足 $m^2 \leq x$.

**思路**

对于非负整数 *m* 来说，由于 *m* 越小越满足$m^2 \leq x$，*m* 越大越不满足$m^2 \leq x$, 有单调性, 可以二分答案。

二分$m$: 

* 如果$m^2 \leq x$成立，那么最终答案大于等于 *m*，更新 *left*。
* 如果$m^2 > x$么最终答案小于 m，更新 *right*。

最后，讨论二分的上下界。**本文用开区间二分，其他二分写法也是可以的。**

开区间二分左边界：0。题目保证$x \geq 0$，所以$(x + 1)^2 > x$一定成立。

开区间二分右边界：$x+1$。题目保证 $x \geq 0$，所以 $(x+1)^2 >x$ 一定成立。

开区间二分右边界（优化）：$min(x,46340)+1$，其中 $46340=\left\lfloor \sqrt{2^{31}-1} \right\rfloor$. 因为题目保证 $x \leq 2^{31} - 1 = 2147483647$ , 所以 $46341^2 =2147488281>x$ 一定成立。

**答疑**

问：为什么代码用 int 类型就够了？为什么不会溢出？

答：在循环中，m 的值始终在二分区间范围内，也就是说 $0<m<46341$，所以 $m^2\leq 46340^2
 =2147395600$，在 32 位有符号整数的范围内，所以不会溢出。

问：为什么代码返回的是 $left$？

答：在练习二分时，请注意「求最小」和「求最大」的二分写法上的区别。

「求最小」的题目和二分查找求「排序数组中某元素的第一个位置」是类似的，按照 红蓝染色法，左边是不满足要求的（红色），右边则是满足要求的（蓝色）。

「求最大」的题目（例如本题）则相反，左边是满足要求的（蓝色），右边是不满足要求的（红色）。这会导致二分写法和「求最小」有一些区别。

以开区间二分为例：

求最小：check(mid) == true 时更新 right = mid，反之更新 left = mid，最后返回 right。
求最大：check(mid) == true 时更新 left = mid，反之更新 right = mid，最后返回 left。

对于开区间写法，简单来说 check(mid) == true 时更新的是谁，最后就返回谁。相比其他二分写法，开区间写法不需要思考加一减一等细节，推荐使用开区间写二分。

```java
class Solution {
    private static final int SQRT_INT_MAX = (int) Math.sqrt(Integer.MAX_VALUE);
    public int mySqrt(int x) {
        // 开区间 (left, right)
       int left = 0;
       int right = Math.min(x, SQRT_INT_MAX) + 1;

       while(left + 1 < right){
        // 开区间不为空
        // 循环不变量: left^2 <= x
        // 循环不变量: right^2 > x
        int mid = left + (right-left) /2 ;
        if (mid * mid <= x){
            left = mid;
        }else{
            right = mid;
        }
       } 
       // 循环结束时 left + 1 == right
       // 此时left^2 <= x 且right^2 > x
       // 所以left最大的满足m^2 <= x的数
        return left;
    }

}

// TC: O(logmin(x,U))，其中 U=46340 为二分区间的最大长度。
// SC: O(1)
```

