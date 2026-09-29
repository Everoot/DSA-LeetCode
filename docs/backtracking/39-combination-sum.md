---
tags:
    - Array
    - Backtracking
    - Google
    - Top Interviews
---

# [39. Combination Sum](https://leetcode.com/problems/combination-sum/)

Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of* `candidates` *where the chosen numbers sum to* `target`*.* You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

 

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
```

**Example 3:**

```
Input: candidates = [2], target = 1
Output: []
```

**Solution:**

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> subResult = new ArrayList<>();

        dfs(0, candidates, target, subResult, result);
        return result; 
    }

    private void dfs(int index, int[] candidates, int target, List<Integer> subResult, List<List<Integer>> result){
        if (target == 0){
            result.add(new ArrayList<>(subResult));
            return;
        }

        if (index == candidates.length || target < 0){
            return;
        }

        dfs(index + 1, candidates, target, subResult, result);

        subResult.add(candidates[index]);
        dfs(index, candidates, target - candidates[index], subResult, result);
        subResult.remove(subResult.size() - 1);
    }
}
// TC: O(2^target) O(N^{T/(M+1)})
// SC: O(target) / O(T/M) 
```



```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> subResult = new ArrayList<>();

        dfs(0, candidates, target, subResult, result);
        return result; 
    }

    private void dfs(int index, int[] candidates, int target, List<Integer> subResult, List<List<Integer>> result){
        if (target == 0){
            result.add(new ArrayList<>(subResult));
            return;
        }

        if (index == candidates.length || target < candidates[index]){
            return;
        }

        dfs(index + 1, candidates, target, subResult, result);

        subResult.add(candidates[index]);
        dfs(index, candidates, target - candidates[index], subResult, result);
        subResult.remove(subResult.size() - 1);
    }
}
```



## 剪枝优化

把 candidates 从小到大排序，如果递归中发现 left<candidates[i]，由于后面的数字只会更大，所以无法把 left 减小到 0，可以直接返回.

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();

        List<Integer> subResult = new ArrayList<Integer>();

        int curSum = 0;
        int index = 0;
        backtracking(index, target, candidates, curSum, subResult, result);
        return result;

    }

    private void backtracking(int index, int target, int[] candidates, int curSum, List<Integer> subResult, List<List<Integer>> result){
        // base case
        if (index == candidates.length){
            if (curSum == target){
                result.add(new ArrayList<Integer>(subResult));
                return;
            }
            return;
        }

        if (curSum > target){
            return;
        }

        int cur = candidates[index];

        // branch
        int maxTake = (target - curSum) / cur;
        for (int i = 0; i <= maxTake; i++){
            for(int j = 0; j < i; j++){
                subResult.add(cur);
            }
            backtracking(index + 1, target, candidates, curSum + cur * i, subResult, result);
            for (int k = 0; k < i; k++){
                subResult.remove(subResult.size() - 1);
            }

        }
    }
}
// k = candidates.length
// level = Math.max(target/Math.min(candidates[i]))
// TC: O(branch^level) = O(k^n)
// SC: O(target/Math.min(candidates[i]))
```







follow up how to improve?



![JPEG image-41F3-8BC1-E4-0](./39. Combination Sum/JPEG image-41F3-8BC1-E4-0.jpeg)

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        if (candidates == null || candidates.length == 0){
            return result;
        }

        int index = 0;
        List<Integer> subResult = new ArrayList<Integer>();
        helper(candidates, target, index, result, subResult);
        return result;
    }

    private void helper(int[] candidates, int target, int index, List<List<Integer>> result,
    List<Integer> subResult  ){
        if (index == candidates.length){
            if (target == 0){
                result.add(new ArrayList<Integer>(subResult));// 
            }
            return;
        }

        int maxBranch = target/candidates[index] + 1;
        // 7 /2 = 3         0 x 2 1 x2 2 x2 3x2

        for (int i = 0; i < maxBranch; i++){
            for (int j = 0; j < i; j++){
                subResult.add(candidates[index]);
            }

            helper(candidates, target - candidates[index]*i, index+1, result, subResult);
            for (int j = 0; j < i; j++){
                subResult.remove(subResult.size() -1);
            }

        }
    }
}

// TC: O(branch^level) = O(B^n) = O((target/min+1) ^ n) = O(target^n)
// SC: O(level)
```

<img src="./39. Combination Sum/JPEG image-4932-87F4-66-0.jpeg" alt="JPEG image-4932-87F4-66-0" style="zoom: 67%;" />

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        if (candidates == null || candidates.length == 0){
            return result;
        }
        List<Integer> subresult = new ArrayList<Integer>();
        helper(candidates, target, 0, subresult, result);

        return result;
    }


    private static void helper(int[] candidates, int targetLeft, int index, List<Integer> subresult, List<List<Integer>> result){
        if (index == candidates.length){
            if (targetLeft == 0){
                result.add(new ArrayList(subresult));
            }
            return;
        }
        
        int MaxNumber = targetLeft / candidates[index];
        for (int i = 0; i <= MaxNumber; i++){
            for (int j = 0; j < i; j++){
                subresult.add(candidates[index]);
            }
            helper(candidates, targetLeft - i * candidates[index], index + 1, subresult, result);


            for (int j = 0; j < i; j++){
                subresult.remove(subresult.size()-1);
            }
        }
    }

}




/*

level: only think about one candidates

branch:       

level                        7
                           / \  \       \     
0                   2*0      2*1   2*2   2*3
                 /   \   \
1               3*0  3*1  3*2


*/
```



use 

```mermaid
graph TD
    A("Start: 4") -->|0x2| B("4")
    A -->|1x2| C("2")
    C -->|0x3| D("2")
    C -->|1x3| E("-1")
    D -->|0x4| F("2")
    D -->|1x4| G("-2")
    F -->|1x2| H("0")
    B -->|0x3| I("4")
    B -->|1x3| J("1")
    I -->|0x4| K("4")
    I -->|1x4| L("0")
    J -->|0x4| M("1")
    M -->|1x3| N("-8")
    A -->|2x2| O("0")
    A -->|1x4| Q("0")
    
    H(["Valid: [2,2]"])
    L(["Valid: [3,1]"])
    O(["Valid: [2,2]"])
    Q(["Valid: [4]"])



```

TC: O(branch^level) = O(B^n) = O((target/min+1) ^ n) = O(target^n)

SC: O(level)

### DFS 经典例题3 Print all combinations of coins that can sum up to a total value n.

73. Combinations Of Coins

Given a number of different denominations of coins (e.g., 1 cent, 5 cents, 10 cents, 25 cents), get all the possible ways to pay a target number of cents.

**Arguments**

- coins - an array of positive integers representing the different denominations of coins, there are no duplicate numbers and the numbers are sorted by descending order, eg. {25, 10, 5, 2, 1}
- target - a non-negative integer representing the target number of cents, eg. 99

**Assumptions**

- coins is not null and is not empty, all the numbers in coins are positive
- target >= 0
- You have infinite number of coins for each of the denominations, you can pick any number of the coins.

**Return**

- a list of ways of combinations of coins to sum up to be target.
- each way of combinations is represented by list of integer, the number at each index means the number of coins used for the denomination at corresponding index.

**Examples**

coins = {2, 1}, target = 4, the return should be

[

 [0, 4],  (4 cents can be conducted by 0 * 2 cents + 4 * 1 cents)

 [1, 2],  (4 cents can be conducted by 1 * 2 cents + 2 * 1 cents)

 [2, 0]   (4 cents can be conducted by 2 * 2 cents + 0 * 1 cents)

]



E.g. total value n = 99 cents

coin value(币值) = 25 10 5 1 cent

one possible way:

3 * 25 = 75 cents						75

2*10 = 20 cents 						 95

0* 5 = 0 cents							 95

4 * 1 = 4 cents							99



[25, 10, 5, 1]

[3, 2, 0, 4]						OK

[0, 0, 0, 99]					  also OK



DFS基本方法:

1. what does it store on each level? (每层代表什么意义? 一般来讲解题之前就知道DF要recurse多少层)

   每一层  选一个coin     

   最多多少层: 99

2. How many different states should we try to put on this level? (每层有多少个状态/ case 需要try?) 

   4 branches     有几个硬币有几个分支



Method 1 : 对的, 但是不那么好

target = 99

​																	target = 99

​							/ 25							 /10 						\ 5									\1

level 0			25(rem = 74)			 10(rem=89)			  5(rem = 94)				  4(rem = 98)   

​						/			|		\		\

level 1          25(49)  10(64) 5(69)  1(73)            ....



......

level 99          1 (rem = 0)



branch = 4 

level = 99

Time = O( 4^99)

Space = O(99)    // 容易爆栈了

   

   ![Screen Shot 2022-05-19 at 01.30.22](./39 Combination Sum.assets/Screen Shot 2022-05-19 at 01.30.22.png)

有重复





如何去掉重复, 压缩层数

99->98->97->96 -> ..... 1



Solution: 



每一层只考虑一种硬币

分支: 拿几个

 

​																			target = 99

​													/ 0     			     |1              	         |2    				    \ 3

level 0: 25							99						  74							49							24

​							/0   /1  /2  /3 |4|5 |6|7  \8 \9

level 1: 10        							

level 2: 

level 3:

层数 = 硬币的种类数

branch: 99

level: 44





![Screen Shot 2022-05-19 at 23.35.41](./39 Combination Sum.assets/Screen Shot 2022-05-19 at 23.35.41.png)

Time: O(99^4)

Space: O(4)



​										Solution 1												vs                           Solution 2

Time:							O(4 ^ 99)												>								O(99^4)

Space:						level is not decided before running 										level is always 4

Deduplication:			need to worry																			worry-free



```java
void findCombianation(int[] coins, int moneyLeft, int index, int[] sol){
  if (index == 3){
    sol[index] = moneyLeft;
    print sol..
    return;
  }
  // 对于这一层要考虑的硬币 coins[index]最多能拿多少
  // i 代表我们拿了几个 coins[index]
  for (int i = 0; i <= maxWeCanTakeAtThisLevel; i++){
    sol[index] = i;
    findCombianation(coins, moneyLeft - i*coins[index], index +1,sol)
    sol[index] = 0;
  }
}
```

​						初始:	   [0,  0, 0 , 0]

​										[25, 10, 5, 1]		remain Target			level第几层: 告诉你当前层正在考虑哪儿个硬币

​													|						|							|

```java
void findCombination(int[] coin, int moneyLeft, int index, int[] sol){
  if (index == 3){ // 必须建立在最后为1 直接掉过最后一层
    sol[index] = moneyLeft;
    print solution and return
  }
  // money value on this level == coin[index];
  // branch的含义: 这种硬币选几个
  // 这一层是哪儿种硬币 coins[index]
  // 最多选: moneyLeft/coins[index]
  // i: 这一次尝试选了i个, 
  for (int i = 0; i <= moneyLeft / coin[index]; i++); {
    sol[index] = i;
    findCombination(coin, moneyLeft - i * coin[index], index + 1, sol);
    
    //sol[index] = 0; 吐
    
    // 这里的index 是下一次要考虑哪儿个硬币  index 是控制硬币种类.
  }
}
```



i 1 = 50 个1

solution[index] = 50 然后去下一层

findCombination()

//我应该把它还原成0

solution[index] = 0      // 为什么这行可以不写 因为可以覆盖掉



i 2 = 51

solution[index] = 51 