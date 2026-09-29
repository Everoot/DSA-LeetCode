---
tags:
    - Array
    - Dynamic Programming
    - Greedy
    - Top Interviews
---

#[55. Jump Game](https://leetcode.com/problems/jump-game/)

You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return `true` *if you can reach the last index, or* `false` *otherwise*.

 

**Example 1:**

```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2:**

```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```



**Solution:**

先看示例 2，nums=[3,2,1,0,4]。我们在遍历数组的同时，维护最右可以到达的位置 mx，如下表：

| *i*  | $nums_i$ | $i+nums_i$ | *mx* |
| ---- | -------- | ---------- | ---- |
| 0    | 3        | 3          | 3    |
| 1    | 2        | 3          | 3    |
| 2    | 1        | 3          | 3    |
| 3    | 0        | 3          | 3    |
| 4    | 4        | 8          | 失败 |

从 0 可以跳到 1,2,3，但是无法从 1,2,3 中的任何位置跳到 4。当我们遍历到 i=4 时，发现
$$
i>mx
$$

这意味着 i 是无法到达的，返回 false。

然后来看示例 1，nums=[2,3,1,1,4]，在遍历数组的同时，维护最远可以到达的位置 mx，如下表：

| *i*  | $nums_i$ | $i+nums_i$ | *mx* |
| ---- | -------- | ---------- | ---- |
| 0    | 2        | 2          | 2    |
| 1    | 3        | 4          | 4    |
| 2    | 1        | 3          | 4    |
| 3    | 1        | 4          | 4    |
| 4    | 4        | 8          | 8    |

从 0 可以跳到 1,2，最远可以到达的位置 mx=2。能否跳到更远的位置？那就看从 1 能跳到哪些位置，从 2 能跳到哪些位置。

从 1 可以跳到 2,3,4，mx 更新成 4。

从 2 可以跳到 3，mx 不变。

从 3 可以跳到 4，mx 不变。

到达 4，返回 true。

一般地，算法如下：

1. 从左到右遍历 nums，同时维护能跳到的最远位置 mx，初始值为 0。
2. 如果 i>mx，说明无法跳到 i，返回 false。
3. 否则，用 $i+nums_i$ 更新 mx 的最大值。
4. 如果循环中没有返回 false，那么最后返回 true。

另一种理解方式是，把每个 $nums_i$看成闭区间$[i,i+nums_i]$，问题变成判定这 n 个区间能否合并成一个大区间（而不是多个区间），这可以用 56. 合并区间 的 算法 解决。



```java
class Solution {
    public boolean canJump(int[] nums) {
        int mx = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i > mx) { // 无法到达 i
                return false;
            }
            mx = Math.max(mx, i + nums[i]); // 从 i 最右可以跳到 i + nums[i]
        }
        return true;
    }
}
// TC: O(n)
// SC: O(1)
```

也可以在 mx≥n−1 时就返回 true，这可以让我们提前退出循环。

```java
class Solution {
    public boolean canJump(int[] nums) {
        int mx = 0;
        for (int i = 0; mx < nums.length - 1; i++) {
            if (i > mx) { // 无法到达 i
                return false;
            }
            mx = Math.max(mx, i + nums[i]); // 从 i 最右可以跳到 i + nums[i]
        }
        return true;
    }
}
// TC: O(n)
// SC: O(1)
```



```java
class Solution {
    public boolean canJump(int[] nums) {
        // base case
        if (nums == null || nums.length <= 0){
            return false;
        }

        boolean[] M = new boolean[nums.length];
        M[nums.length -1] = true;

        for (int i = nums.length - 2; i >= 0; i--){
            for (int j = nums.length -1; j > i; j--){
                if (M[j] == true && nums[i] + i >= j){
                    M[i] = true;
                    break;
                }
            }
        }
        return M[0];
    }
}



/*

// TC: O(n^2)
// SC: O(n)

boolean M[nums.length]
M[nums.length - 1] = true 

index.   0  1  2  3  4
M[]               T  T             M[i]: represent whether can jump from i to the end 
        [2, 3, 1, 1, 4]  
               i                   i start from nums.length - 2
                     j                j start from nums.length - 1 

induction rule: 
if (M[j] == true && nums[i] + i >= j)
M[i] = true;
break;

return: M[0]


*/
```



<---------------------------------------------------------------------------------------------------------------------------------------------------------

| index      i | 0                      | 1                      | 2                                         | 3                                                            | 4                   |
| ------------ | ---------------------- | ---------------------- | ----------------------------------------- | ------------------------------------------------------------ | ------------------- |
| input        | 2                      | 3                      | 0                                         | 1                                                            | 4                   |
| M[i]         | T<br />可以跳到t的地方 | T<br />可以跳到t的地方 | F<br />我只能跳0步<br />即不能跳到T的地方 | T<br />在这儿的时候, <br />往右看,我只能跳一步 <br />能否跳到一个t的地方 | T<br />出生就在罗马 |

**Base case**: M[length - 1] = true, because it's target itself.

**Induction rule:**

M[i] represents whether I can jump from the i-th element to the target. (能否从i跳到尾)	
$$
M[i] = true \ \ if \ \ \exists_j \ M[j] == true \ OR  \ \ i + A[i] \geqslant target
$$
i < j <= i + input[i]

如果我能从i跳到某个中继站j而且已经知道j能够到达终点了

false 	otherwise

**Return M[0]**

linear scan 回头看

外面是个O(n) 里面也是O(n)

TC: O(n^2)

Space: O(n)





### Solution 2: 从左往右 

但比第一种差

index  = 0	1	2	3	4

input = [2,  3,    1,    1,   4]

M[]     = T    T	 T	T	T



前面有人自己是T 还能跳到我的 填T

base case: M[0] = true

M[i] represents whether I can jump from the start element to the i-th element. (能否从0跳到i)



linear scan 回头看 



Time: $O(n^2)$



