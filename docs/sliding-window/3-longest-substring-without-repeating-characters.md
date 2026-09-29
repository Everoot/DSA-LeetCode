---
tags:
  - Hash Table
  - String
  - Sliding Window
  - Microsoft
  - Top Interviews
---

# [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

Given a string `s`, find the length of the longest **substring** without repeating characters.

**Example 1:**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 2:**

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

**Solution:**

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Set<Character> set = new HashSet<>();

        int n = s.length();

        int left = 0;
        int result = 0;

        for (int right = 0; right < n; right++){
            char cur = s.charAt(right);
            if (set.contains(cur)){
                while(s.charAt(left) != cur){
                    set.remove(s.charAt(left));
                    left++;
                }
                left++;
            }else{
                // set not contains the cur
                set.add(cur);
            }
            result = Math.max(result, right - left + 1);
        }


        return result;
    }
}



/*
Sliding window
      0 1 2 
      a b c a b c b b 
        l   r
      ------

global result = tracking window size

1. add right (for loop)
    2. remove left
        if (set.contains(cur)){
            there is repeat happened. 
            remove left from set

            while(cur != s.charAt(left)){
            }

        }
    3. update result 

tool: set<Character> : store the current window content (make sure not repeat)
int left = window left side (include)
int right = window right side (include)

cur window size = (r - l + 1) = (2 - 0 + 1) = 3 


// TC: O(n)
// SC: O(n)


*/
```



```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int maxLength = 1;

        int right = 0;
        int left = 0;
        int n = s.length();

        Map<Character, Integer> map = new HashMap<>();

        while(right < n){
            char curRight = s.charAt(right);
            map.put(curRight, map.getOrDefault(curRight, 0) + 1);

            while(map.get(curRight) > 1){
                char curLeft = s.charAt(left);
                int curLeftCount = map.get(curLeft);
                map.put(curLeft, curLeftCount - 1);
                left++;
            }

            maxLength = Math.max(maxLength, right - left + 1);

            right++;
        }

        return maxLength;

    }
}

// TC: O(n)
// SC: O(1) ASCII -> O(128) -> O(1)
```



```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Set<Character> set = new HashSet<>();
        int result = 0;
        int left = 0;
        for (int right = 0; right < s.length(); right++){ 
            char cur = s.charAt(right);
            if (!set.contains(cur)){
                set.add(cur);
            }else{
                while(s.charAt(left) != cur){
                    set.remove(s.charAt(left));
                    left++;
                }

                left++;
            }

            int curLen = right - left + 1;

            result = Math.max(curLen, result);
        }

        return result;
    }
}
```



```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int result = 0;
        Map<Character, Integer> map = new HashMap<>();
        int left = 0;
        for (int right = 0; right < s.length(); right++){
            char cur = s.charAt(right);
            while(map.getOrDefault(cur, 0) > 0){
                map.put(s.charAt(left), map.get(s.charAt(left)) - 1);
                left++;
            }

            map.put(cur, 1);

            result = Math.max(result, right - left + 1);

            
        }
        return result;
    }
}

// TC: O(n)
// SC: O(1)
```



```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s == null){
            return 0;
        }

        if (s.length() <= 1){
            return s.length();
        }

        Set<Character> set = new HashSet<Character>();
        int result = 0;

        int slow = 0;
        int fast = 0;

        while (fast < s.length()){
            if (set.contains(s.charAt(fast))){
                set.remove(s.charAt(slow));
                slow++;
            }else{
                set.add(s.charAt(fast));
                result = Math.max(result, fast - slow + 1);
                fast++;

            }
        }
        return result;
    }
}
//TC: O(n)
//SC: O(n)
```



```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s == null || s.length() == 0){
            return 0;
        }
        Map<Character, Integer> map = new HashMap<Character, Integer>();

        int slow = 0;
        int result = 0;
        for (int fast = 0; fast < s.length(); fast++){
            // Step 1: add fast
            map.put(s.charAt(fast), map.getOrDefault(s.charAt(fast), 0) + 1);

            // Step 2: remove slow
            if (map.size() < fast - slow + 1){
                int count = map.get(s.charAt(slow));
                if (count == 1){
                    map.remove(s.charAt(slow));
                }else{
                    map.put(s.charAt(slow), count - 1);
                }

                slow++;
            }

            // Step 3:
            int curLen = fast - slow + 1;
            if (map.size() == curLen){
                result = Math.max(result, curLen);
            }
        }

        return result;
    }
}

/*
// TC: O(n)
// SC: O(n)


            abcabcbb
            s
               f

    map:
    <a,1>
    <b,1>
    <c,1>


*/
```

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        // base case
        if (s == null ){
            return 0;
        }

        if (s.length() <= 1){
            return s.length();
        }

        Set<Character> c = new HashSet<Character>();
        int longest = 0;
        int slow = 0;
        int fast = 0;
        while(fast < s.length()){
            if (c.contains(s.charAt(fast))){
                // exist
                c.remove(s.charAt(slow));
                slow++;
            }else{
                // not exist
                c.add(s.charAt(fast));
                fast++;
                longest = Math.max(longest, fast - slow);
            }
        }

        return longest;

    }
}

// TC: O(n)

// SC: O(n)



/*
    set:  c
    longest = 3
    a   b   c   a   b   c   b   b
    while(f < s.length)
                                s
                                   f

    1. check set
        1.1 if exit
        remove slow
        slow++
        1.2 if not exit
            :   1.2.1: add.set
                1.2.2: fast++;
                1.2.3: Math(longest, new fast - s);


*/
```





```typescript
function lengthOfLongestSubstring(s: string): number {
    let result : number = 0;
    let left : number = 0;

    const set : Set<string> = new Set<string>;
    const len : number = s.length;
    for (let right : number = 0; right < len; right++){
        const cur : string = s[right];
        if (!set.has(cur)){
            set.add(cur);
        }else{
            while(s[left] !== cur){
                set.delete(s[left]);
                left++;
            }
            left++;
        }

        const curLen : number = right - left + 1;

        result = Math.max(curLen, result);
    }

    return result;
};
```

>  1. **`const` 用在：**
>
> ✅ **永远不会被重新赋值的变量**。
>
> 比如：
>
> - 常量
>- 不可变的对象引用
> - 函数表达式
> 
> ```typescript
>const pi = 3.14159;
> const name = "Bingying";
> const nums = [1, 2, 3];  // 这个数组可以改内容，但 `nums` 这个引用不会变。
> ```
> 
> ⚡ 注意：
>
> - `const` 保证 **变量指向的内存地址不会变**，但是如果是对象/数组，它的**内部内容**可以改。
>- 比如：
> 
> ```typescript
>const arr = [1, 2, 3];
> arr.push(4); // ✅ 可以改数组内容
> // arr = [5, 6, 7]; // ❌ 错误，不能重新赋值
> ```
> 
> 2. **`let` 用在：**
>
> ✅ **值需要改变**的变量。
>
> 比如：
>
> - 循环中的变量
>- 滑动窗口的 slow/fast
> - 需要重新赋值的变量
> 
> ```typescript
>let count = 0;
> count = count + 1; // 需要更新值，所以用 let
> ```
> 
> 常见场景：
>
> ```typescript
>for (let i = 0; i < 10; i++) {
>     console.log(i);
> }
> ```
> 
> 3. **不要用 `var`！**
>
> 以前 JavaScript 用 `var`，但是 `var` 有很多**奇怪的问题**（比如不会 block scope），所以
> 🚫 在 TypeScript（或现代JavaScript）里，**几乎不用 `var` 了**，只用 `let` 和 `const`。
> 
> **小总结（背下来！）**
>
> | 关键词  | 什么时候用         | 特点         |
>| ------- | ------------------ | ------------ |
> | `const` | 绝大部分时候用它   | 不能重新赋值 |
> | `let`   | 需要重新赋值时用它 | 可以改值     |
> | `var`   | 不要用             | 会有坑       |



https://youtu.be/JiITYolxnQU

