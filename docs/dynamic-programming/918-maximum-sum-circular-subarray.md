---
tags:
    - Array
    - Divide and Conquer
    - Dynamic Programming
    - Queue
    - Monotonic Queue		
    - Weekly Contest 105
    - Top Interviews
---

# [918. Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)

Given a **circular integer array** `nums` of length `n`, return *the maximum possible sum of a non-empty **subarray** of* `nums`.

A **circular array** means the end of the array connects to the beginning of the array. Formally, the next element of `nums[i]` is `nums[(i + 1) % n]` and the previous element of `nums[i]` is `nums[(i - 1 + n) % n]`.

A **subarray** may only include each element of the fixed buffer `nums` at most once. Formally, for a subarray `nums[i], nums[i + 1], ..., nums[j]`, there does not exist `i <= k1`, `k2 <= j` with `k1 % n == k2 % n`.

 

**Example 1:**

```
Input: nums = [1,-2,3,-2]
Output: 3
Explanation: Subarray [3] has maximum sum 3.
```

**Example 2:**

```
Input: nums = [5,-3,5]
Output: 10
Explanation: Subarray [5,5] has maximum sum 5 + 5 = 10.
```

**Example 3:**

```
Input: nums = [-3,-2,-3]
Output: -2
Explanation: Subarray [-2] has maximum sum -2.
```

 

**Constraints:**

- `n == nums.length`
- `1 <= n <= 3 * 104`
- `-3 * 104 <= nums[i] <= 3 * 104`



Solution:

**分类讨论一:** 

| 5    | -9   | <span style="color:blue">4</span> | <span style="color:blue">4</span> | -9   | 5    |
| ---- | ---- | --------------------------------- | --------------------------------- | ---- | ---- |

子数组没有跨过边界(在nums中间). 此时算的是最大非空子数组和(leetcode 53), 记作maxS.



**分类讨论二:**

| <span style="color:blue">5</span> | -9   | 4    | 4    | -9   | <span style="color:blue">5</span> |
| --------------------------------- | ---- | ---- | ---- | ---- | --------------------------------- |

子数组跨过边界(在nums两端). 由于子数组和+其余元素和=sum(nums)=常数, 所以其余元素和越小, 子数组和越大.

算出最小子数组和minS, 从sum(nums)中减去minS, 就得到了跨过边界的最大子数组和. 这两类讨论取得最大值, 答案就是max{maxS, sum(nums) - minS}

| 5    | <span style="color:red">-9</span> | <span style="color:red">4</span> | <span style="color:red">4</span> | <span style="color:red">-9</span> | 5    |
| ---- | --------------------------------- | -------------------------------- | -------------------------------- | --------------------------------- | ---- |



做完了吗? 还有一种特殊情况没有考虑. 如果最小子数组就是整个数组, 那么跨过边界的最大子数组是空的, 这是题目不允许的. 如何判断这种特殊情况呢?

❌:

| <span style="color:red">-9</span> | <span style="color:red">-9</span> | <span style="color:red">4</span> | <span style="color:red">4</span> | <span style="color:red">-9</span> | <span style="color:red">-9</span> |
| --------------------------------- | --------------------------------- | -------------------------------- | -------------------------------- | --------------------------------- | --------------------------------- |

✅:

判断依据是minS= sum(nums), 此时只能返回maxS(最大子数组在nums中间).

**问**：为什么当 *minS*=sum(nums) 时，最小子数组可以是整个数组？

答：用反证法证明。假设最小子数组一定不是整个数组，这意味着 nums 的某个前缀或者后缀是大于 0 的（包含这个前缀/后缀会让 minS 变大），所以 minS<sum(nums)，矛盾。所以当 minS=sum(nums) 时，最小子数组可以是整个数组。

> 注：对于 nums=[−1,1,−1]，最小子数组可以取 [−1]，也可以取整个数组 [−1,1,−1]。对于这样的 nums，最大子数组一定不会跨过边界，只返回 maxS 仍然是正确的。

```java
class Solution {
    public int maxSubarraySumCircular(int[] nums) {
        int maxF = 0; // 计算最大子数组和的 DP 数组（空间优化成一个变量）
        int maxS = Integer.MIN_VALUE; // 最大子数组和，不能为空
        int minF = 0; // 计算最小子数组和的 DP 数组（空间优化成一个变量）
        int minS = 0; // 最小子数组和，可以为空（元素和为 0）
        int sum = 0; // nums 的元素和

        for (int x : nums) {
            // 53. 最大子数组和（空间优化写法）
            maxF = Math.max(maxF, 0) + x;
            maxS = Math.max(maxS, maxF);
            minF = Math.min(minF, 0) + x;
            minS = Math.min(minS, minF);
            sum += x;
        }

        return sum == minS ? maxS : Math.max(maxS, sum - minS);
    }
}
// TC: O(n)
// SC: O(1)
```





环形数组 有两种情况

1. 不环, 和53一样 直接取最大子数组
2. 环起来, 总和 - 最小子数组

特例全是负数->直接返回最大值

```java
class Solution {
    public int maxSubarraySumCircular(int[] nums) {
       if (nums == null || nums.length == 0){
        return 0;
       }

       int total = nums[0];
       // 和 53 题一样：最大子数组
       int maxResult = nums[0];
       int maxChildchoose = nums[0];
        // 新增：最小子数组
       int minResult = nums[0];
       int minChildchoose = nums[0];

       for (int i = 1; i < nums.length; i++){
        int cur = nums[i];

        total = cur + total;
            // 继承 or 放弃（最大）
        maxChildchoose = Math.max(maxChildchoose + cur, cur);
        maxResult = Math.max(maxChildchoose, maxResult);
          // 继承 or 放弃（最小）
        minChildchoose = Math.min(minChildchoose + cur, cur);
        minResult = Math.min(minChildchoose, minResult);
       }
        // 全负情况：不能用环形
       if (maxResult < 0){
        return maxResult;
       }
     // 普通最大 vs 环形最大
       return Math.max(maxResult, total - minResult);
    }
}

// TC: O(n)
// SC: O(1)
```

