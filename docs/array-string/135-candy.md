---
tags:
    - Array
    - Greedy
    - Top Interviews
---



#[135. Candy](https://leetcode.com/problems/candy/)

There are `n` children standing in a line. Each child is assigned a rating value given in the integer array `ratings`.

You are giving candies to these children subjected to the following requirements:

- Each child must have at least one candy.
- Children with a higher rating get more candies than their neighbors.

Return *the minimum number of candies you need to have to distribute the candies to the children*.

 

**Example 1:**

```
Input: ratings = [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.
```

**Example 2:**

```
Input: ratings = [1,2,2]

Output: 4
Explanation: You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
The third child gets 1 candy because it satisfies the above two conditions.
```

 

**Constraints:**

- `n == ratings.length`
- `1 <= n <= 2 * 104`
- `0 <= ratings[i] <= 2 * 104`



Solution:

Identify the Constraints: 

"Each child must have at least one candy.", "Children with a higher rating get more candies than their neighbors.", These constraints make it challenging to adjust candies directly for each child, as increasing candies for one child could lead to violations in neighboring conditions.

Intuitive Greedy Strategy:

For such problems, a greedy approach often works well. We can ensure that each child meets the conditions by doing two passes: one from left to right and another from right to left.

The core of the greedy approach here is to focus on local conditions for each child and its neighbors, without revisiting or readjusting already valiated children.

```java
class Solution {
    public int candy(int[] ratings) {
        int n = ratings.length;
        int[] candies = new int[n];

        // Step 1: Give each child at least 1 candy
        for (int i = 0; i < n; i++){
            candies[i] = 1;
        }

        // Step 2: Left-to-right pass
        for (int i = 1; i < n; i++){
            if (ratings[i - 1] < ratings[i]){
                candies[i] = candies[i - 1] + 1;
            }
        }

        // Step 3: Right-to-left pass
        for (int i = n - 2; i >= 0; i--){
            if (ratings[i] > ratings[i + 1]){
                candies[i] = Math.max(candies[i], candies[i + 1] + 1);
            }
        }
      // 1 2 3 2 1
      // 

        // Step 4: Sum up all the candies
        int totalCandies = 0;
        for (int candy : candies){
            totalCandies = candy + totalCandies;
        }

        return totalCandies;
    }
}

// TC: O(n)
// SC: O(n)
```

分析
题目要求，每个孩子至少分 1 颗糖果。我们先给每个孩子 1 颗糖果，下面就不用考虑这个要求了，可以给孩子最少 0 颗糖果。

题目要求，相邻两个孩子评分更大的孩子会获得更多的糖果。如何理解这句话呢？

比如 ratings=[1,2,3,4,5,4,3,2]，由严格递增段 [1,2,3,4]、峰顶 5 和严格递减段 [4,3,2] 组成。

对于严格递增段，最左边的孩子旁边没有评分比他小的孩子，所以给他 0 颗糖果就行（注意我们在一开始给了每个孩子一颗糖果），下一个孩子的糖果必须比上一个孩子的多，贪心地，多 1 就行。所以分给严格递增段的糖果数为 0,1,2,3。
对于严格递减段，最右边的孩子旁边没有评分比他小的孩子，所以给他 0 颗糖果就行（注意我们在一开始给了每个孩子一颗糖果），往左看，左边孩子的糖果必须比右边孩子多，贪心地，多 1 就行。所以分给严格递减段的糖果数为 2,1,0。
最后确定峰顶的糖果数，必须比严格递增段的最大值多，即 ≥4；也必须比严格递减段的最大值多，即 ≥3，取 max(4,3)=4 作为峰顶的糖果数。
把严格递增段 + 峰顶 + 严格递减段叫做一座山。

如果数组中只有一座山，计算糖果总数是简单的。但如果有多座山呢？如何找到每座山？

分组循环
适用场景：按照题目要求，数组会被分割成若干组，每一组的判断/处理逻辑是相同的。

核心思想：

外层循环负责遍历组之前的准备工作（记录开始位置），和遍历组之后的统计工作（更新答案）。
内层循环负责遍历组，找出这一组最远在哪结束。
这个写法的好处是，各个逻辑块分工明确，也不需要特判最后一组（易错点）。以我的经验，这个写法是所有写法中最不容易出 bug 的，推荐大家记住。

详细计算过程
以 ratings=[1,2,3,4,2,1,4,5,2] 为例说明：

* 第一组，先上坡再下坡，即 [1,2,3,4,2,1]。
  * 记录起始下标 start=i=0。
  * 写一个循环找严格递增段，如果 ratings[i]<ratings[i+1] 就继续循环。循环结束时 i 就是峰顶下标，记作 top。这里 top=3。
  * 写一个循环找严格递减段，如果 ratings[i]>ratings[i+1] 就继续循环。循环结束时 i 就是严格递减段最后一个数的下标。这里 i=5。
  * 设 inc=top−start。从 start 到 top−1，分到的糖果数依次为 0,1,2,…,inc−1（注意我们在一开始给了每个孩子一颗糖果），根据等差数列求和公式，得糖果总数为  $\frac{inc(inc - 1)}{2}$ 。这里 inc=3，糖果总数为 0+1+2=3。
  * 设 dec=i−top。从 top+1 到 i 到，分到的糖果数依次为 dec−1,dec−2,…,0，根据等差数列求和公式，得糖果总数为 $\frac{dec(dec - 1)}{2}$. 这里 dec=2，糖果总数为 0+1=1。
  * 峰顶的糖果数为 max(inc,dec)=3。
  * 把 i 加一，找下一组。
* 第二组，先上坡再下坡，即 [1,4,5,2]。做法同上。
  * 注意这里的 1 也是前一组的最后一个数（谷底会被两组共享）。在记录 start 时，如果 i>0 且 ratings[i−1]<ratings[i]，说明 i−1 是谷底，记录 start=i−1，否则记录 start=i。
  * ⚠注意：我们在一开始给了每个孩子一颗糖果，在后面分组循环的计算过程中，不考虑一开始分的这颗糖果。第一组的最后一个数虽然也在第二组的第一个数中，但由于这个孩子在分组循环中分到的糖果数是 0，所以不会重复统计。

对于 ratings=[1,2,2,3] 这种包含相邻相同元素的情况，按照上文的规则，分成两组 [1,2] 和 [2,3]。糖果数为 0,1,0,1。

特别地，如果一组的第一个数是峰顶，则 inc=0；如果一组的最后一个数是峰顶，则 dec=0。

示例 1 ratings=[1,0,2]，分成两组 [1,0] 和 [0,2]。答案为 3（先给每个孩子一颗糖果）加上 1+0 加上 0+1 等于 5。

示例 2 ratings=[1,2,2]，分成两组 [1,2] 和 [2]。答案为 3（先给每个孩子一颗糖果）加上 0+1 加上 0 等于 4。

总结
先给每个孩子一颗糖果，即初始化答案为数组长度 n。然后用分组循环计算每一组（严格递增段 + 峰顶 + 严格递减段）的 inc 和 dec，把答案增加
$$
\frac{inc(inc−1)+dec(dec−1)}{2} + max(inc, dec)
$$
