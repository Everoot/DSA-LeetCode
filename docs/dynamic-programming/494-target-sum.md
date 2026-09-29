---
tags:
    - Array
    - Dynamic Programming
    - Backtracking
    - Top Interview 150
---

# [494. Target Sum](https://leetcode.com/problems/target-sum/) ❤️‍🔥

You are given an integer array `nums` and an integer `target`.

You want to build an **expression** out of nums by adding one of the symbols `'+'` and `'-'` before each integer in nums and then concatenate all the integers.

- For example, if `nums = [2, 1]`, you can add a `'+'` before `2` and a `'-'` before `1` and concatenate them to build the expression `"+2-1"`.

Return the number of different **expressions** that you can build, which evaluates to `target`.

 

**Example 1:**

```
Input: nums = [1,1,1,1,1], target = 3
Output: 5
Explanation: There are 5 ways to assign symbols to make the sum of nums be target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```

**Example 2:**

```
Input: nums = [1], target = 1
Output: 1
```

 

**Constraints:**

- `1 <= nums.length <= 20`
- `0 <= nums[i] <= 1000`
- `0 <= sum(nums[i]) <= 1000`
- `-1000 <= target <= 1000`



**Solution:**

假设所有数的和为$s$, 添加正数的和$p$, 那么负数的和为$-(s-p)$, 那么$p - (s- p) = t$. 把该式子化简下, 可得$2p-s=t$, 即$p =(s+t)/2$

因为可以被除以2, 所以p一定是个偶数, 且p因为是正数, 所以s+t也一定是正数

运筹学:

**01 背包问题** 是一个经典的组合优化问题，其核心是如何在资源有限的情况下选择最优的方案以最大化收益

问题描述:

你有一个固定容量的背包，和 nnn 件物品，每件物品都有：

- 重量 $w[i]$
- 价值 $v[i]$

目标是： 在不超过背包容量$W$的前提下，选择若干物品放入背包，使得背包中物品的总价值最大化.



决策变量: 

* 定义$x[i] \in {0, 1}$ , 表示是否选择物品$i$:

  * $x[i]=1$: 表示选择物品$i$
  * $x[i] = 0$: 表示不选择物品$i$

设$x[i]$表示是否选择物品$i$, 其中: 
$$
x[i] \in {0,1}, i=1,2,\cdots,n
$$


目标函数:
$$
Z = \max\sum_{i = 1}^{n}v[i]\cdot x[i]
$$
约束条件:
$$
\sum_{i = 1}^{n}w[i]\cdot x[i] \leq W
$$
动态规划

状态定义:

令$dp[j]$ 表示在容量$j$的背包下能获得的最大值

状态转移方程: 
$$
dp[j] = max(dp[j], dp[j - w[i]] + v[i])
$$
这里的$dp[j -w[i]] + v[i]$ 表示选择当前物品$i$ 后的价值, 取两者中的最大值

边界条件:
$$
dp[0]=0
$$

```java
public class knapsack{
  public static int knapsack(int[] weights, int values, int W){
    int n = weights.length;
    int[] dp = new int[W + 1];
    
    for (int i = 0; i < n; i++){
      for (int j = W; j >= weights[i]; j--){
        dp[j] = math.max(dp[j], dp[j - weights[i]] + values[i]);
      }
    }
    
    return dp[W];
  }
  
  public static void main(String[] args) {
        // 测试用例
        int[] weights = {2, 3, 4, 5};
        int[] values = {3, 4, 5, 8};
        int W = 8;

        int maxValue = knapsack(weights, values, W);
        System.out.println("最大价值: " + maxValue); // 输出: 最大价值: 11
  }
}

// TC: O(n x W), 其中$n$是物品数量, W是背包容量.
// SC: O(W)
```



#### 0-1 Knapsack

0-1 背包: 有$n$ 个物品, 第$i$ 个物品的体积为$w[i]$, 价值为$v[i]$, 每个物品至多选一个, 求体积和不超过capacity时的最大价值和

回溯三问:

1. 当前操作? 枚举第$i$ 个物品选或不选: 

   不选, 剩余容量不变; 选, 剩余容量减少$w[i]$

2. 子问题? 在剩余容量为c时, 从前$i$ 个物品中得到的最大价值和

3. 下一个子问题? 分类讨论:

   不选: 在剩余容量为$c$ 时, 从前$i - 1$ 个物品中得到的最大价值和

   选: 在剩余容量为$c - w[i]$ 时, 从前$i - 1$个物品中得到的最大价值和

$$
dfs(i, c) = max(dfs(i - 1, c), dfs(i - 1, c - w[i]) + v[i])
$$

```java
public class knapsack{
  public static int knapsack(int[] weights, int[] values, int c){
    int n = weights.length;
    
    return dfs(n - 1, weights, values, c);
    
  }
  
  private static int dfs(int i, int[] weights, int[] values, int c){
    if (i < 0){
      return 0;
    }
    
    if (weights[i] > c){
      // 体积超过当前容量了 -> 不选
      return dfs(i - 1, weights, c);
    }
    // 选
    return Math.max(dfs(i - 1, weights, c), dfs(i - 1, weights, c - weights[i]) + values[i]);
    
  }
  
  public static void main(String[] args) {
        // 测试用例
        int[] weights = {2, 3, 4, 5};
        int[] values = {3, 4, 5, 8};
        int W = 8;

        int maxValue = knapsack(weights, values, W);
        System.out.println("最大价值: " + maxValue); // 输出: 最大价值: 11
  }
}

// TC: O(n x c), 其中c是物品数量, c是背包容量.
// SC: O(c)
```



```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        // p (添加+的他们的和) 
        // s - p  (添加-的和) s 所有元素的和
        // p - (s- p) =t
        // 2p = s+t
        // p = (s +t)/2

        int sum = 0;
        for (int i = 0; i < nums.length; i++){
            sum = nums[i] + sum;
        }

        sum = sum + target;

        if (sum < 0 || sum % 2 != 0){
            return 0;
        }

        sum = sum / 2;

        int n = nums.length;

        return zero_one_knapsack(sum, nums);
        
    }

    private int zero_one_knapsack(int target, int[] nums){
        int n = nums.length;
        return dfs(n - 1, target, nums);
    }

    private int dfs(int i, int target, int[] nums){
        if (i < 0){
            // no thing 
            if (target == 0){
                return 1; 
            }else{
                return 0; // 不合法
            }
        }

        if (target < nums[i]){
            return dfs(i - 1, target, nums);
        }

        return dfs(i - 1, target, nums) + dfs(i - 1, target - nums[i], nums);
    }   

    // capacity: 背包容量
    // w[i]: 第i个物品的体积
    // v[i]: 第i个物品的价值
    // 返回: 所选物品体积和不超过capacity的前提下, 所能得到的最大价值和

    // private int zero_one_knapsack(int capacity, int[] w, int[] nums){
    //     int n = nums.length;
    //     dfs(n - 1, capacity, w, nums)
    // }

    // private void dfs(int i, int c, int[] w,  int[] v){
    //     if (i < 0){
    //         // no thing 
    //         return 0;
    //     }

    //     if (c < w[i]){
    //         return dfs(i - 1, c, w, v);
    //     }

    //     return Math.max(dfs(i - 1, c, w, v), dfs(i - 1, c - w[i], w, v) + v[i]);
    // }
}
```



```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        // goal: return the number of different expressions. 
        // method: dfs 

        int n = nums.length;
        // t
        // p + (-m) = t
        // m = p - t
        // s = sum(nums)
        // m = s -p
        // p + (-(s - p)) = t
        // 2p = t + s
        // p > 0
        // p = 
        int sum = 0;
        for (int i = 0; i < nums.length; i++){
            sum = sum + nums[i];
        } 

        sum = sum + target;

        if (sum < 0 || sum % 2 != 0){
            return 0;
        }

        sum  = sum/ 2;

        return dfs(n - 1, sum, nums);
    }

    private int dfs(int i, int target, int[] nums){
        if (i < 0){
            if (target == 0){
                return 1;
            }else{
                return 0;
            }
        }

        if (target < nums[i]){
            return dfs(i - 1, target, nums);
        }

        return dfs(i - 1, target, nums) + dfs(i - 1, target - nums[i], nums);
    }
}
// TC: O(2^n)
// SC: O(n)
```



### 1. Backtrack

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        // s+ t
        int sum = 0;
        for (int i = 0; i < nums.length; i++){
            sum = sum + nums[i];
        }

        // s+ t
        target = target + sum;

        if (target < 0 || target % 2 != 0){
            return 0;
        }

        // p = (s +t)/2
        target = target/2;

        int n = nums.length; 
        return dfs(n - 1, nums, target);
    }

    private int dfs(int i, int[] nums, int target){
        if (i < 0){
            if (target == 0){
                return 1;
            }else{
                return 0;
            }
        }

        if (nums[i] > target){
            return dfs(i - 1, nums, target);
        }

        return dfs(i - 1, nums, target) + dfs(i - 1 , nums, target - nums[i]);

    }
}
```



```java
class Solution {
    int count = 0;
    public int findTargetSumWays(int[] nums, int target) {
        int index = 0;
        int sum = 0;

        dfs(nums, index, sum, target);
        return count;
    }

    private void dfs(int[] nums, int index, int sum, int target){
        if (index == nums.length){
            if (sum == target){
                count++;
            }

            return;
        }

        dfs(nums, index + 1, sum + nums[index], target);

        dfs(nums, index + 1, sum - nums[index], target);

    }
}


// TC: O(branch^level) = O(2^n)
// SC: O(n)

/*

                    1 1 1 1 1

index i           1 
               +/      \-
              1+1      1-1
              /\ 


index 

index: means current nums[index]


*/

```



```java
class Solution {
    int count = 0;
    public int findTargetSumWays(int[] nums, int target) {
        int index = nums.length - 1;
        
        int sum = 0;
        for (int i = 0; i < nums.length; i++){
            sum = sum + nums[i];
        }

        target = sum + target;
        if (target < 0 || target % 2 != 0){
            return 0;
        }

        target = target /2;

        int result = dfs(nums, index, target);
        return result;
    }

    private int dfs(int[] nums, int index, int target){
        if (index < 0){
            if (target == 0){
                return 1;
            }
            return 0;
        }

        if (target < nums[index]){ // 超过背包容量了
            return dfs(nums, index - 1, target);
        }

        return dfs(nums, index - 1, target) + dfs(nums, index - 1, target - nums[index]);

    }
}
```



### 2. 递归搜索 + 保存计算结果 = 记忆化搜索

设 $nums$ 的元素和为 $s$，其中添加正号的元素之和为 $p$，其余添加负号的元素（绝对值）之和为 $q$，那么有
$$
p+q=s
$$

> ```
> Input: nums = [1,1,1,1,1], target = 3
> 
> Input: nums = [1,1,1,1,1], target = 3
> Output: 5
> Explanation: There are 5 ways to assign symbols to make the sum of nums be target 3.
> -1 + 1 + 1 + 1 + 1 = 3
> +1 - 1 + 1 + 1 + 1 = 3
> +1 + 1 - 1 + 1 + 1 = 3
> +1 + 1 + 1 - 1 + 1 = 3
> +1 + 1 + 1 + 1 - 1 = 3
> 
> s =  1 + 1 + 1 + 1 + 1 = 5
> -1 + 1 + 1 + 1 + 1 = 3
> |-1| + |1 + 1 + 1 + 1| = 5
>  q   +   p      = 5
>  1 + 4 = 5
> 
> p - q = target
> 4 - 1 = 3
> ```

又因为表达式运算结果等于 $target$，所以有
$$
p−q=target
$$
解得
$$
\begin{cases}
 &  p = \frac{s + target}{2}\\
 &  q = \frac{s - target}{2} 
\end{cases}
$$
这意味着，只要我们选出的取正号的元素和恰好等于$p$，或者取负号的元素和恰好等于$q$，就等价于表达式的结果恰好等于 $target$. 所以问题变成从 nums 中选一些数，使得这些数的元素和恰好等于 $p$（或者 $q$）的方案数.

* 如果 $target \geq 0$，那么取 $q=\frac{s−target}{2}$ 作为背包容量比 $p$ 更好.
* 如果$target < 0$, 那么取$p = \frac{s + target}{2}$作为背包容量比$q$ 更好.

综上所述, 取
$$
\frac{s - |target|}{2}
$$
作为0-1背包的背包容量是最优的. (注意target 可以是负数)

如果 $s−|target|$ 是奇数，那么$\frac{s - target}{2}$ 是小数. 由于无法从整数中选出一些数，元素和等于小数，所以当 $s -|target|$ 是奇数的时候，方案数是 $0$，直接返回$0$就行.

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        // s+ t
        int sum = 0;
        for (int i = 0; i < nums.length; i++){
            sum = sum + nums[i];
        }

        // s+ t
        target = target + sum;

        if (target < 0 || target % 2 != 0){
            return 0;
        }

        // p = (s +t)/2
        target = target/2;

        int n = nums.length; 
        int[][] memo = new int[n][target + 1];
        for (int[] row : memo){
            Arrays.fill(row, -1);
        }
        return dfs(n - 1, nums, target, memo);
    }

    private int dfs(int i, int[] nums, int target, int[][] memo){
        if (i < 0){
            if (target == 0){
                return 1;
            }else{
                return 0;
            }
        }

        if (memo[i][target] != -1){
            return memo[i][target];
        }

        if (nums[i] > target){
            memo[i][target] = dfs(i - 1, nums, target, memo);
            return memo[i][target];
        }

        memo[i][target] = dfs(i - 1, nums, target, memo) + dfs(i - 1 , nums, target - nums[i], memo);
        return memo[i][target];

    }
}
// TC: O(nm) // m = target
// SC: O(nm)
// 状态个数(n*target) 乘以每个状态所需的时间O(1)
```

### 3.  1:1 Translation to Recurrence Form

Common variations:

* **At most** fill the capacity → find the number of ways / the maximum total value
* **Exactly** fill the capacity → find the number of ways / the maximum or minimum total value
* **At least** fill the capacity → find the number of ways / the minimum total value

$$
dfs(i, c) = dfs(i - 1, c)+ dfs(i - 1, c - w[i])
$$

$$
f[i][c] = f[i - 1][c] + f[i - 1][c-w[i]]\\
f[i + 1][c] = f[i][c] + f[i][c - w[i]]
$$

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        // s+ t
        int sum = 0;
        for (int i = 0; i < nums.length; i++){
            sum = sum + nums[i];
        }

        // s+ t
        target = target + sum;

        if (target < 0 || target % 2 != 0){
            return 0;
        }

        // p = (s +t)/2
        target = target/2;

        int n = nums.length; 

        int[][] dp = new int[n + 1][target + 1];
        dp[0][0] = 1; // 初始化
      // 递归的边界条件已经告诉你了 
      /*
      	if (i < 0){
            if (target == 0){
                return 1;
            }else{
                return 0;
            }
        }
      */
        for (int i = 0; i < n; i++){
            for (int c = 0; c <= target; c++){
                if (c < nums[i]){
                    dp[i + 1][c] = dp[i][c]; // 只能不选
                }else{
                    dp[i + 1][c] = dp[i][c] + dp[i][c-nums[i]];
                  // 不选 + 选
                }
            }
        }

        return dp[n][target];
    }

}

// TC: O(nm)
// SC: O(nm)
```



```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        // s+ t
        int sum = 0;
        for (int i = 0; i < nums.length; i++){
            sum = sum + nums[i];
        }

        // s+ t
        target = target + sum;

        if (target < 0 || target % 2 != 0){
            return 0;
        }

        // p = (s +t)/2
        target = target/2;

        int n = nums.length; 

        int[][] dp = new int[n][target + 1];
        
        for (int i = 0; i <= target; i++){
            if (nums[0] == i){
                dp[0][i] = 1;
            }
        }
        dp[0][0]++;
        for (int i = 1; i < n; i++){
            for (int c = 0; c <= target; c++){
                if (c < nums[i]){
                    dp[i][c] = dp[i - 1][c];
                }else{
                    dp[i][c] = dp[i - 1][c] + dp[i-1][c-nums[i]];
                }
            }
        }

        return dp[n-1][target];
    }

}

// 如果要写第一个维度是 n 而不是 n+1 的 DP 数组，一定要注意 nums[0] = 0 的选或不选，这会导致 dp[0][0]=2。所以推荐第一个维度是 n+1 的写法。
```



```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        // goal: return the number of different expressions. 
        // method: dfs 

        int n = nums.length;
        // t
        // p + (-m) = t
        // m = p - t
        // s = sum(nums)
        // m = s -p
        // p + (-(s - p)) = t
        // 2p = t + s
        // p > 0
        // p = 
        int sum = 0;
        for (int i = 0; i < nums.length; i++){
            sum = sum + nums[i];
        } 

        sum = sum + target;

        if (sum < 0 || sum % 2 != 0){
            return 0;
        }

        sum  = sum/ 2;

        // int[][] memo = new int[n][sum + 1];
        // for (int[] row : memo){
        //     Arrays.fill(row, -1);
        // }

        int[][] memo = new int[n + 1][sum+1];

        memo[0][0] = 1;
        for (int i = 0; i < n; i++){
            for (int c = 0; c <= sum; c++){
                if (c < nums[i]){
                    memo[i + 1][c] = memo[i][c];
                }else{
                    memo[i + 1][c] = memo[i][c] + memo[i][c - nums[i]];
                }
            }
        }

        return memo[n][sum];

        // return dfs(n - 1, sum, nums, memo);
    }

    private int dfs(int i, int target, int[] nums, int[][] memo){
        if (i < 0){
            if (target == 0){
                return 1;
            }else{
                return 0;
            }
        }

        if (memo[i][target] != -1){
            return memo[i][target];
        }


        if (target < nums[i]){
            memo[i][target] = dfs(i - 1, target, nums, memo);
            return memo[i][target];
        }

        memo[i][target] = dfs(i - 1, target, nums, memo) + dfs(i-1, target - nums[i], nums, memo);

        return memo[i][target];
    }
}
```





----

### 4. 空间优化: 两个数组(滚动数组)

每次把$f(i+1)[c]$ 算完之后 后面就不会用到f[i]了. 也就是说, 每时每刻只有两个数组中的元素在参与状态转移, 那么干脆就只用两个数组, 比如你把f[1]算完了, 那么在计算f[2]的时候, 我就直接把结果填到f[0]当中, 我们计算f[3]的时候, f[1]没用, 我就直接把结果填到f[1]里面, 那怎么用代码实现呢. 我们把所有的i和i+1都模2就好了, 比如i等于1的时候, 就是从f[1]转移到f[0]这个数组上, i =2的时候, 就是从f[0]这个数组转移到f[1]这个数组上.



```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        // s+ t
        int sum = 0;
        for (int i = 0; i < nums.length; i++){
            sum = sum + nums[i];
        }

        // s+ t
        target = target + sum;

        if (target < 0 || target % 2 != 0){
            return 0;
        }

        // p = (s +t)/2
        target = target/2;

        int n = nums.length; 
        // int[][] memo = new int[n][target + 1];
        // for (int[] row : memo){
        //     Arrays.fill(row, -1);
        // }
        // return dfs(n - 1, nums, target, memo);

        int[][] dp = new int[2][target+1];
        dp[0][0] = 1;
        for (int i = 0; i < n; i++){
            for (int c = 0; c <= target; c++){
                if (c < nums[i]){
                    dp[(i + 1)%2][c] = dp[i%2][c];
                }else{
                    dp[(i + 1)%2][c] = dp[i%2][c] + dp[i%2][c-nums[i]];
                }
            }
        }

        return dp[n%2][target];
    }
}
// TC: O(nm)
// SC: O(m)
```



### 空间优化: 一个数组

$$
f[i + 1][c] = f[i][c] + f[i][c - w[i]]
$$

$$
f(c) = f(c) + f[c-w[i]]
$$

| i    | 1    | 2    | 3    | 5    | 6    | 7    | 9    | w[i]=2 |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ------ |
| i+1  | 1    | 2    | 3    | 5    | 6    | 7    | 9    |        |
|      |      |      | 4    | 7    | 9    | 12   | 15   |        |

 

倒着算

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        // s+ t
        int sum = 0;
        for (int i = 0; i < nums.length; i++){
            sum = sum + nums[i];
        }

        // s+ t
        target = target + sum;

        if (target < 0 || target % 2 != 0){
            return 0;
        }

        // p = (s +t)/2
        target = target/2;

        int n = nums.length; 

        int[] dp = new int[target + 1];
        dp[0] = 1;

        for (int i = n - 1; i >= 0; i--){
            for (int c = target; c >= nums[i]; c--){
                dp[c] = dp[c] + dp[c - nums[i]];
            }
        }
        
        return dp[target];
    }
}

// TC: O(nm)
// SC: O(m)
```

????
