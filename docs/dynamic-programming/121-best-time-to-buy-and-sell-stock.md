---
tags:
    - Array
    - Dynamic Programming
    - Top Interviews
---

# [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return *the maximum profit you can achieve from this transaction*. If you cannot achieve any profit, return `0`.

 

**Example 1:**

```
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
```

**Example 2:**

```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.
```

 

**Constraints:**

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`



**Solution:**

```java
class Solution {
    public int maxProfit(int[] prices) {
        int result = 0;
        int minPrice = prices[0];

        for (int p : prices){
            result = Math.max(result, p - minPrice);
            minPrice = Math.min(minPrice, p);
        }

        return result;
    }
}

// TC: O(n)
// SC: O(1)
```

低价买入, 高价卖出

从左到右枚举卖出价格 prices[i]，那么要想获得最大利润，我们需要知道第 i 天之前，股票价格的最小值是什么，也就是从 prices[0] 到 prices[i−1] 的最小值，把它作为买入价格，这可以用一个变量 minPrice 维护。

请注意，minPrice 维护的是 prices[i] 左侧元素的最小值。

由于只能买卖一次，所以在遍历中，维护 prices[i]−minPrice 的最大值，就是答案。

```java
class Solution {
    public int maxProfit(int[] prices) {
        int result = 0; 
        int n = prices.length;

        // for (int i = 0; i < n; i++){
        //     // buy
        //     int buy = prices[i];
        //     for (int j = i + 1; j < n; j++){
        //         // sell
        //         int sell = prices[j];
        //         int profit = sell - buy;
        //         result = Math.max(profit, result); 
        //     }
        // }

        // return result;
        int curMAX = prices[n-1];
        for (int i = n - 2; i >= 0; i--){
            int buy = prices[i];
            int profit = curMAX - buy;
            curMAX = Math.max(buy, curMAX);
            result = Math.max(result, profit); 
        }

        return result;
    }
}

// TC: O(n)
// SC: O(1)
```



Slide window:

```java
class Solution {
    public int maxProfit(int[] prices) {
        int l = 0;
        int len = prices.length;
        int maxPro = 0;
        for (int r=0; r<len; r++){
            if (prices[l]<prices[r]){
                maxPro = Math.max(maxPro, (prices[r]-prices[l]));
            }else{
                l = r;
            }
        }
        return maxPro;
    }
}
```



```java
class Solution {
    public int maxProfit(int[] prices) {
        int result = 0;

        int min = Integer.MAX_VALUE;

        for (int i = 0; i < prices.length; i++){
            min = Math.min(min, prices[i]);

            result = Math.max(result, prices[i] - min);
        }


        return result;
    }
}

// TC: O(n)
// SC: O(1)
```



```java
class Solution {
    public int maxProfit(int[] prices) {
        int result = 0;
        int min = Integer.MAX_VALUE;
        for (int i = 0; i < prices.length; i++){
            if (prices[i] < min){
                min = prices[i];
            }else if (prices[i] - min > result){
                result = prices[i] - min;
            }
        }

        return result;


    }
}

/*  [7,1,5,3,6,4] 
       i          

*/

// TC: O(n)
// SC: O(1)
```

注意题意理解 