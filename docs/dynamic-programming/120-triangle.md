---
tags:
    - Array
    - Dynamic Programming
    - Top Interviews
---

# [120. Triangle](https://leetcode.com/problems/triangle/)

Given a `triangle` array, return *the minimum path sum from top to bottom*.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index `i` on the current row, you may move to either index `i` or index `i + 1` on the next row.

**Example 1:**

```
Input: triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
Output: 11
Explanation: The triangle looks like:
   2
  3 4
 6 5 7
4 1 8 3
The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).
```

**Example 2:**

```
Input: triangle = [[-10]]
Output: -10
```

 

**Constraints:**

- `1 <= triangle.length <= 200`
- `triangle[0].length == 1`
- `triangle[i].length == triangle[i - 1].length + 1`
- `-104 <= triangle[i][j] <= 104`

 

**Follow up:** Could you do this using only `O(n)` extra space, where `n` is the total number of rows in the triangle?



**Solution:**

## 一、寻找子问题

由于 *triangle* 每排的下标都是从 0 开始的，示例 1 的正确图示应该是**左对齐**，即
$$
\begin{align*}
&2\\
&3 4\\
&6 5 7\\
&4 1 8 3\\
\end{align*}
$$
我们要解决的问题(原问题)是:

* 从最上面的$(0,0)$出发, 移动到$triangle$的最后一排, 路径上的元素之和的最小值.

考虑下一步往哪走:

* 走到$(1,0)$, 那么需要解决的问题为: 从$(1,0)$出发, 移动到$triangle$最后一排, 路径上的元素之和的最小值.
* 走到$(1,1)$, 那么需要解决的问题为: 从$(1,1)$出发, 移动到$triangle$最后一排, 路径上的元素之和的最小值.

这些问题都是和原问题相似的, 规模更小的子问题, 可以用递归解决.

## 二、状态定义与状态转移方程

根据上面的讨论, 定义状态为$dfs(i, j)$, 表示从$(i, j)$ 出发, 移动到$triangle$最后一排, 路径上的元素之和的最小值.

考虑下一步往哪走:

* 走到$(i + 1, j)$, 那么需要解决的问题为: 从$(i+1, j)$出发, 移动到$triangle$ 最后一排, 路径上的元素之和的最小值, 即$dfs(i + 1, j)$.
* 走到$(i+1, j+1)$, 那么需要解决的问题为: 从$(i + 1, j +1)$出发, 移动到$triangle$最后一排, 路径上的元素之和的最小值, 即$dfs(i+1, j+1)$.

这两种情况取最小值, 再加上当前位置的元素值$triangle[i][j]$就得到了$dfs(i,j)$, 即
$$
dfs(i, j) = min(dfs(i + 1, j), dfs(i + 1, j + 1)) + triangle[i][j]
$$
递归边界: $dfs(n-1, j) = triangle[n-1][j]$. 走到最后一排就无法再走了, 路径上只有一个元素$triangle[n-1][j]$.

递归入口: $dfs(0,0)$, 这是原问题, 也是答案.

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        return dfs(0,  0, triangle);
    }

    private int dfs(int i, int j, List<List<Integer>> triangle){
        if(i == triangle.size() - 1){
            return triangle.get(i).get(j);
        }

        return Math.min(dfs(i + 1, j, triangle), dfs(i + 1, j + 1, triangle)) + triangle.get(i).get(j);


    }
}
// TC: O(2^n)
// SC: O(n)
```

## 三、递归搜索 + 保存递归返回值 = 记忆化搜索

考虑到整个递归过程中有大量重复递归调用（递归入参相同）。由于递归函数没有副作用，同样的入参无论计算多少次，算出来的结果都是一样的，因此可以用**记忆化搜索**来优化：

* 如果一个状态（递归入参）是第一次遇到，那么可以在返回前，把状态及其结果记到一个 memo 数组中。
* 如果一个状态不是第一次遇到（memo 中保存的结果不等于 memo 的初始值），那么可以直接返回 memo 中保存的结果。

注意：memo 数组的初始值一定不能等于要记忆化的值！例如初始值设置为 0，并且要记忆化的 dfs(i,j) 也等于 0，那就没法判断 0 到底表示第一次遇到这个状态，还是表示之前遇到过了，从而导致记忆化失效。一般把初始值设置为 −1。本题由于 triangle[i][j] 可以是负数，所以改用 $-\infty$作为初始值。

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[][] memo = new int[n][n];
        for (int[] row : memo){
            Arrays.fill(row, Integer.MIN_VALUE);
        }
        return dfs(0,  0, triangle, memo);
    }

    private int dfs(int i, int j, List<List<Integer>> triangle, int[][] memo){
        if(i == triangle.size() - 1){
            return triangle.get(i).get(j);
        }

        if (memo[i][j] != Integer.MIN_VALUE){
            return memo[i][j];
        }

        return memo[i][j] = Math.min(dfs(i + 1, j, triangle, memo), dfs(i + 1, j + 1, triangle, memo)) + triangle.get(i).get(j);


    }
}

// TC: O(n^2), 其中 n 为 triangle 的长度。由于每个状态只会计算一次，动态规划的时间复杂度 = 状态个数 × 单个状态的计算时间。本题状态个数等于 O(n^2)，单个状态的计算时间为 O(1)，
// SC: O(n^2), 保存多少状态, 就需要多少空间
```

## 四、1:1 翻译成递推

我们可以去掉递归中的「递」，只保留「归」的部分，即自底向上计算。

具体来说，$f[i][j]$ 的定义和 dfs(i,j) 的定义是一样的，都表示从 (i,j) 出发，移动到 triangle 最后一排，路径上的元素之和的最小值。

相应的递推式（状态转移方程）也和 dfs 一样：

$$
f[i][j]=min(f[i+1][j],f[i+1][j+1])+triangle[i][j]
$$
初始值 $f[n−1][j]=triangle[n−1][j]$，翻译自递归边界 $dfs(n−1,j)=triangle[n−1][j]$。

答案为$f[0][0]$，翻译自递归入口 dfs(0,0)。

答疑
问：如何思考循环顺序？什么时候要正序枚举，什么时候要倒序枚举？

答：这里有一个通用的做法：盯着状态转移方程，想一想，要计算 $f[i][j]$，必须先把 $f[i+1][j]$ 和 $f[i+1][j+1]$ 算出来，那么只有 i 从大到小枚举才能做到。对于 j 来说，正序倒序都可以。

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();

        int[][] memo = new int[n][n];

        for (int j = 0; j < n; j++){
            memo[n - 1][j] = triangle.get(n - 1).get(j);
        }

        for (int i = n - 2; i >= 0; i--){
            for (int j = 0; j <= i; j++){
                memo[i][j] = Math.min(memo[i + 1][j], memo[i + 1][j + 1]) + triangle.get(i).get(j);
            }
        }

        return memo[0][0];
    }
}
// TC: O(n^2)
// SC: O(n^2)
```

---

## 五、空间优化

### 写法一

由于 $f[i][j]$ 只依赖正下方和右下方的相邻数据，可以去掉 *f* 的第一个维度。

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[] f = new int[n];
        for (int j = 0; j < n; j++) {
            f[j] = triangle.get(n - 1).get(j);
        }
        for (int i = n - 2; i >= 0; i--) {
            for (int j = 0; j <= i; j++) {
                f[j] = Math.min(f[j], f[j + 1]) + triangle.get(i).get(j);
            }
        }
        return f[0];
    }
}
// TC: O(n^2)
// SC: O(1)
```

### 写法二

也可以直接把 *triangle* 当作 *f* 数组。

```java
class Solution {
    public int minimumTotal(List<List<Integer>> f) {
        for (int i = f.size() - 2; i >= 0; i--) {
            for (int j = 0; j <= i; j++) {
                f.get(i).set(j, f.get(i).get(j) + Math.min(f.get(i + 1).get(j), f.get(i + 1).get(j + 1)));
            }
        }
        return f.get(0).get(0);
    }
}
// TC: O(n^2)
// SC: O(1)
```

