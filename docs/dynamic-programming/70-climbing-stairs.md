---
tags:
    - Math
    - Dynamic Programming
    - Memoization
    - Top Interviews
---

# [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top? 

**Example 1:**

```
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

**Example 2:**

```
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

 

**Constraints:**

- `1 <= n <= 45`



**Solution:**

一、启发思考：寻找子问题
假设 n=9。

我们要解决的问题是从 0 爬到 9 有多少种不同的方法（或者说爬 9 个台阶的方案数）。

分类讨论：

如果最后一步爬了 1 个台阶，那么我们得先爬到 8，要解决的问题缩小成：从 0 爬到 8 有多少种不同的方法。
如果最后一步爬了 2 个台阶，那么我们得先爬到 7，要解决的问题缩小成：从 0 爬到 7 有多少种不同的方法。
由于这两种情况都会把原问题变成一个和原问题相似的、规模更小的子问题，所以可以用递归解决。

注 1：从大往小思考，主要是为了方便把递归翻译成递推。从小往大思考也是可以的。

注 2：动态规划有「选或不选」和「枚举选哪个」两种基本思考方式。在做题时，可根据题目要求，选择适合题目的一种来思考。本题用到的是「枚举选哪个」。

二、递归怎么写：状态定义与状态转移方程
因为要解决的问题都是「从 0 爬到 i」，所以定义 dfs(i) 表示从 0 爬到 i 有多少种不同的方法（或者说爬 i 个台阶的方案数）。

分类讨论：

如果最后一步爬了 1 个台阶，那么我们得先爬到 i−1，要解决的问题缩小成：从 0 爬到 i−1 有多少种不同的方法。
如果最后一步爬了 2 个台阶，那么我们得先爬到 i−2，要解决的问题缩小成：从 0 爬到 i−2 有多少种不同的方法。
由于这两种方法是互相独立的（爬的台阶个数不同），所以根据加法原理，从 0 爬到 i 的方法数等于这两种方法数之和，即

$$
dfs(i)=dfs(i−1)+dfs(i−2)
$$

递归边界：dfs(0)=1, dfs(1)=1。从 0 爬到 0 有一种方法，即原地不动。从 0 爬到 1 有一种方法，即爬 1 个台阶。

递归入口：dfs(n)，也就是答案（爬 n 个台阶的方案数）。

问：为什么 0 到 0 算一种方法？

答：也可以这样理解，如果 dfs(0)=0，那么 dfs(2)=dfs(1)+dfs(0)=1，这显然是错误的，因为有两种方法可以爬到 2。所以（倒推出）dfs(0) 是 1。

```java
// 会超时的递归代码
class Solution {
    public int climbStairs(int n) {
        return dfs(n);
    }

    private int dfs(int i) {
        if (i <= 1) { // 递归边界
            return 1;
        }
        return dfs(i - 1) + dfs(i - 2);
    }
}

```

复杂度分析
时间复杂度：$O(2^n)$。搜索树可以近似为一棵二叉树，树高为 $O(n)$，所以节点个数为 $O(2^n)$，遍历搜索树需要 $O(2^n)$ 的时间。
空间复杂度：$O(n)$。递归需要 $O(n)$ 的栈空间。

## 三、递归 + 记录返回值 = 记忆化搜索

上面的做法太慢了，怎么优化呢？

注意到「先爬 1 个台阶，再爬 2 个台阶」和「先爬 2 个台阶，再爬 1 个台阶」，都相当于爬 3 个台阶，都会从 dfs(i) 递归到 dfs(i−3)。

一叶知秋，整个递归中有大量重复递归调用（递归入参相同）。由于递归函数没有副作用，同样的入参无论计算多少次，算出来的结果都是一样的，因此可以用记忆化搜索来优化：

如果一个状态（递归入参）是第一次遇到，那么可以在返回前，把状态及其结果记到一个 memo 数组中。
如果一个状态不是第一次遇到（memo 中保存的结果不等于 memo 的初始值），那么可以直接返回 memo 中保存的结果。
注意：memo 数组的初始值一定不能等于要记忆化的值！例如初始值设置为 0，并且要记忆化的 dfs(i) 也等于 0，那就没法判断 0 到底表示第一次遇到这个状态，还是表示之前遇到过了，从而导致记忆化失效。一般把初始值设置为 −1。本题由于方案数均为正数，所以可以初始化成 0。

```java
class Solution {
    public int climbStairs(int n) {
        int[] memo = new int[n + 1];
        Arrays.fill(memo, -1);
        return dfs(n, memo);
    }

    private int dfs(int i, int[] memo){
        if (i <= 1){
            return 1;
        }

        if (memo[i] != -1){
            return memo[i];
        }

        memo[i] = dfs(i - 1, memo) + dfs(i - 2, memo);

        return memo[i];
    }
}
```

复杂度分析
时间复杂度：O(n)。由于每个状态只会计算一次，动态规划的时间复杂度 = 状态个数 × 单个状态的计算时间。本题状态个数等于 O(n)，单个状态的计算时间为 O(1)，所以动态规划的时间复杂度为 O(n)。
空间复杂度：O(n)。有多少个状态，memo 数组的大小就是多少。



四、1:1 翻译成递推
我们可以去掉递归中的「递」，只保留「归」的部分，即自底向上计算。

具体来说，f[i] 的定义和 dfs(i) 的定义是一样的，都表示从 0 爬到 i 有多少种不同的方法（或者说爬 i 个台阶的方案数）。

相应的递推式（状态转移方程）也和 dfs 一样：

f[i]=f[i−1]+f[i−2]
相当于之前是用递归去计算每个状态，现在是枚举并计算每个状态。

初始值 f[0]=1, f[1]=1，翻译自递归边界 dfs(0)=1, dfs(1)=1。

答案为 f[n]，翻译自递归入口 dfs(n)。

```java
class Solution {
    public int climbStairs(int n) {
        int[] f = new int[n + 1];
        f[0] = f[1] = 1;
        for (int i = 2; i <= n; i++) {
            f[i] = f[i - 1] + f[i - 2];
        }
        return f[n];
    }
}

```



```java
class Solution {
    public int climbStairs(int n) {
        // int[] memo = new int[n + 1 + 2];
        int[] f = new int[n + 1 + 2];
        // Arrays.fill(memo, -1);
        // return dfs(n, memo);
        f[0] = 1;
        f[1] = 1;
        for (int i = 0; i < n; i++){
            f[i + 2] = f[i+1] + f[i];
        }
        return f[n];
    }

}
```

#### 复杂度分析

- 时间复杂度：O(*n*)。
- 空间复杂度：O(*n*)。



五、空间优化
观察状态转移方程，发现一旦算出 f[i]，那么 f[i−2] 及其左边的状态就永远不会用到了。

这意味着每次循环，只需要知道「上一个状态」和「上上一个状态」的 f 值是多少，分别记作 f1和 f0。它俩的初始值均为 1，对应着 f[1] 和 f[0]。

每次循环，计算出新的状态 newF=f1+f0，那么对于下一轮循环来说：

「上上一个状态」就是 f1，更新 f0=f1。
「上一个状态」就是 newF，更新 f1=newF。最后答案为 f1，因为最后一轮循环算出的 newF 赋给了f1。

```java
class Solution {
    public int climbStairs(int n) {
        int f0 = 1;
        int f1 = 1;
        for (int i = 2; i <= n; i++) {
            int newF = f1 + f0;
            f0 = f1;
            f1 = newF;
        }
        return f1;
    }
}
// TC: O(n)
// SC: O(1)
```



六、矩阵快速幂优化
把状态转移方程用矩阵乘法表示，即
$$
\left [ \begin{matrix} 
	f[i] \\
	f[i-1] 
\end{matrix} \right ] = 
\left [ \begin{matrix} 
	1 & 1 \\
	1 & 0 
\end{matrix} \right ]
\left [ \begin{matrix} 
	f[i - 1] \\
	f[i - 2] 
\end{matrix} \right ]
$$
式中的三个矩阵分别记作 $F[i],M,F[i−1]$，即

$$
F[i]=M×F[i−1]
$$

$$
\begin{align*}
F[n] &= M \times F[n - 1] \\
		 &= M \times M \times F[n-2] \\
		 &= M \times M \times M \times F[n-3] \\
		 &\vdots \\
		 &= M^n \times F[0]
\end{align*}
$$

其中$M^n$可以用快速幂计算, 原理请看[【图解】一张图秒懂快速幂](https://leetcode.cn/problems/powx-n/solution/tu-jie-yi-zhang-tu-miao-dong-kuai-su-mi-ykp3i/)。

初始值

$$
F[0] = \left [ \begin{matrix} 
	f[0] \\
	f[-1] 
\end{matrix} \right ] = 
\left [ \begin{matrix} 
	 1 \\
	 0 
\end{matrix} \right ]
$$
答案为 $f[n]$，即 $F[n]$ 的第一项。

```java
class Solution {
    public int climbStairs(int n) {
        int[][] m = {
            {1, 1},
            {1, 0},
        };

        int[][] f0 = {{1}, {0}};
        int[][] fn = powMul(m, n, f0);
        return fn[0][0];
    }

    // a^n * f0
    private int[][] powMul(int[][] a, int n, int[][] f0){
        int[][] res = f0;
        while(n > 0){
            if ((n & 1) > 0){
                res = mul(a, res);
            }
            a = mul(a, a);
            n >>= 1;
        }

        return res;
    }

    // 返回矩阵a和矩阵b相乘的结果
    private int[][] mul(int[][] a, int[][] b){
        int[][] c = new int[a.length][b[0].length];
        for (int i = 0; i < a.length; i++){
            for (int k = 0; k < a[i].length; k++){
                if (a[i][k] == 0){
                    continue;
                }

                for (int j = 0; j < b[k].length; j++){
                    c[i][j] = c[i][j] + a[i][k] * b[k][j];
                }
            }
        }

        return c;
    }
}
```

#### 复杂度分析

- 时间复杂度：O(log*n*)。
- 空间复杂度：O(1)。

https://leetcode.cn/problems/powx-n/solutions/2858114/tu-jie-yi-zhang-tu-miao-dong-kuai-su-mi-ykp3i/



```java
class Solution {
    public int climbStairs(int n) {
        /*

        dp[i] means how many step to current i staircase
2                _
1           _ 
0        _ 
                 2 + 1
            1+1
dp[i]    1   


        */
        if (n == 1){
            return 1;
        }
        int[] dp = new int[n];

        dp[0] = 1;
        dp[1] = 2;
        for (int i = 2; i < n; i++){
            dp[i] = dp[i - 1] + dp[i - 2];
        }   

        return dp[n - 1];
    }
}

// TC: O(n)
// SC: O(n)
```



Brute Force

```java
class Solution {
    public int climbStairs(int n) {
        return help(0, n);
    }

    public int help(int i, int n){
        if (i > n){
            return 0;
        }

        if (i == n){
            return 1;
        }

        return help(i+1, n) + help(i+2, n);
    }
}

//TLE

//TC: O(2^n)
//SC: O(n)
```



Dynamic:

```java
class Solution {
    public int climbStairs(int n) {
        if (n == 0 || n == 1 || n == 2 || n == 3){
            return  n;
        }

        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 1;
        dp[2] = 2;
        dp[3] = 3;
        for (int i = 4; i < n + 1; i++){
            dp[i] = dp[i-1] + dp[i-2];
        }

        return dp[n];
    }
}


// TC: O(n)
// SC: O(n)

/*
        n = 4

        dp: 0,1,2,3,4
            [0,1,2,3, ]

            dp[i] = dp[i-1] + dp[i-2]


             0 
             / \
           1     2
          / \    / \
         2   3   3  4
       /  \  /   / 
      3   4  4    4
    /
    4         


*/
```





https://leetcode.cn/problems/climbing-stairs/solutions/2560716/jiao-ni-yi-bu-bu-si-kao-dong-tai-gui-hua-7zm1/
