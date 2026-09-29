---
tags:
    - Array
    - Prefix Sum
    - Top Interview 150
---

# [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) 

Given an integer array `nums`, return *an array* `answer` *such that* `answer[i]` *is equal to the product of all the elements of* `nums` *except* `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

 

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

**Example 2:**

```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```

 

**Constraints:**

- `2 <= nums.length <= 105`
- `-30 <= nums[i] <= 30`
- The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

 

**Follow up:** Can you solve the problem in `O(1)` extra space complexity? (The output array **does not** count as extra space for space complexity analysis.)



**Solution:**

answer[i] 等于 nums 中除了 nums[i] 之外其余各元素的乘积。换句话说，如果知道了 i 左边所有数的乘积，以及 i 右边所有数的乘积，就可以算出 answer[i]。

于是：

定义 pre[i] 表示从 nums[0] 到 nums[i−1] 的乘积。
定义 suf[i] 表示从 nums[i+1] 到 nums[n−1] 的乘积。
我们可以先计算出从 nums[0] 到 nums[i−2] 的乘积 pre[i−1]，再乘上 nums[i−1]，就得到了 pre[i]，即

pre[i]=pre[i−1]⋅nums[i−1]
同理有

suf[i]=suf[i+1]⋅nums[i+1]
初始值：pre[0]=suf[n−1]=1。按照上文的定义，pre[0] 和 suf[n−1] 都是空子数组的元素乘积，我们规定这是 1，因为 1 乘以任何数 x 都等于 x，这样可以方便递推计算 pre[1]，suf[n−2] 等。

算出 pre 数组和 suf 数组后，有answer[i]=pre[i]⋅suf[i]

优化前:

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] result = new int[nums.length];        

        int[] prefix = new int[nums.length];
        prefix[0] = 1;
        for (int i = 1; i < nums.length; i++){
            prefix[i] = prefix[i-1] * nums[i-1];
        }

        //[1, 2, 3, 4]
        // 1  1  2  6 

        int[] postfix = new int[nums.length];
        postfix[nums.length -1] = 1;
        for (int i = nums.length -2; i >= 0; i--){
            postfix[i] = nums[i+1] * postfix[i+1];
        } 

        // [1, 2, 3, 4]
        //  24 12   4  1

        result[0] = postfix[0];
        result[nums.length - 1] = prefix[nums.length -1];

        for (int i = 1; i < nums.length-1; i++){
            result[i] = prefix[i] * postfix[i];
        }
    

        return result;
    }
}

    // TC: O(n)
    // SC: O(n)
```



```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] pre = new int[n];
        int[] suf = new int[n];
        pre[0] = 1;
        suf[n - 1] = 1;
        for (int i = 1; i < n; i++){
            pre[i] = pre[i - 1] * nums[i - 1];
        }

        for (int i = nums.length - 2; i >= 0; i--){
            suf[i] = suf[i + 1] * nums[i + 1];
        }

        for (int i = 0; i < n; i++){
            nums[i] = pre[i] * suf[i];
        }  

        return nums;
    }
}
```

优化：不使用额外空间
先计算 suf，然后一边计算 pre，一边把 pre 直接乘到 suf[i] 中。最后返回 suf。

题目说「输出数组不被视为额外空间」，所以该做法的空间复杂度为 O(1)。此外，这种做法比上面少遍历了一次。

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] suf = new int[n];
        suf[n - 1] = 1;

        for (int i = n - 2; i >= 0; i--){
            suf[i] = suf[i + 1] * nums[i + 1];
        }

        int pre = 1;
        for (int i = 0; i < n; i++){
            // 此时pre为nums[0]到nums[i - 1]的乘积, 直接乘到suf[i]中
            suf[i] = suf[i] * pre;
            pre = pre * nums[i];
        }

        return suf;
    }
}
// 复杂度分析
// 时间复杂度：O(n)，其中 n 是 nums 的长度。
// 空间复杂度：O(1)。返回值不计入。

```

