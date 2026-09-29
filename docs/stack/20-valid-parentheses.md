---
tags:
    - String
    - Stack
    - Top Interviews
---

# [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

 

**Example 1:**

```
Input: s = "()"
Output: true
```

**Example 2:**

```
Input: s = "()[]{}"
Output: true
```

**Example 3:**

```
Input: s = "(]"
Output: false
```

**Solution:**

题意解读
什么情况下是有效字符串？
可以从消消乐的角度理解，每次可以消除一对相邻的匹配括号，不断消除，如果可以把 s 变成空字符串，则 s 是有效字符串。

比如 (), (()), [()], [()]{} 等等，都可以通过消除，把 s 变成空字符串。例如

[()]→[]→空串
什么情况下是无效字符串？
左括号没有对应的右括号。例如 (()，缺失了一个右括号。
右括号没有对应的左括号。例如 ())，缺失了一个左括号。
括号类型不匹配。例如 [()}，其中 [ 要和 } 组成一对括号，但是括号类型不同。
思路
本题是「邻项消除」问题（见文末的题单），这类问题都可以用栈解决。

以 s={[()]} 为例说明：

创建一个空栈。
从左到右遍历 s。
s[0]={，这是一个左括号，入栈。
s[1]=[，这是一个左括号，入栈。
s[2]=(，这是一个左括号，入栈。
s[3]=)，这是一个右括号，它必须和栈顶的 ( 组成一对（消除），弹出栈顶。
s[4]=]，这是一个右括号，它必须和栈顶的 [ 组成一对（消除），弹出栈顶。
s[5]=}，这是一个右括号，它必须和栈顶的 { 组成一对（消除），弹出栈顶。
遍历结束，由于栈为空，说明所有括号均已匹配完毕，返回 true。反之，如果在遍历的过程中，发现栈为空，或者括号类型不匹配的情况，返回 false。此外，如果遍历结束栈不为空，说明还有没匹配的左括号，返回 false。
细节
由于括号两两一对，所以 s 的长度必须是偶数。如果 s 的长度是奇数，可以直接返回 false。
我们可以创建一个哈希表（或者数组），保存每个右括号对应的左括号，这样可以直接判断栈顶的左括号是否与右括号为同一类型，从而省去大量 if-else 判断。



method 1: 

```java
class Solution {
    public boolean isValid(String s) {
        if (s.length() % 2 != 0) { // s 长度必须是偶数
            return false;
        }
        Map<Character, Character> mp = new HashMap<>() {{
            put(')', '(');
            put(']', '[');
            put('}', '{');
        }};
        Deque<Character> st = new ArrayDeque<>();
        for (char c : s.toCharArray()) {
            if (!mp.containsKey(c)) { // c 是左括号
                st.push(c); // 入栈
            } else if (st.isEmpty() || st.pop() != mp.get(c)) { // c 是右括号
                return false; // 没有左括号，或者左括号类型不对
            }
        }
        return st.isEmpty(); // 所有左括号必须匹配完毕
    }
}


```



method2:

也可以在哈希表/数组中保存每个左括号对应的右括号。在遍历到左括号时，把对应的右括号入栈。这样遍历到右括号时，只需看栈顶括号是否一样即可。

```java
class Solution {
    public boolean isValid(String s) {
        if (s.length() % 2 != 0) { // s 长度必须是偶数
            return false;
        }
        Map<Character, Character> mp = new HashMap<>() {{
            put('(', ')');
            put('[', ']');
            put('{', '}');
        }};
        Deque<Character> st = new ArrayDeque<>();
        for (char c : s.toCharArray()) {
            if (mp.containsKey(c)) { // c 是左括号
                st.push(mp.get(c)); // 入栈
            } else if (st.isEmpty() || st.pop() != c) { // c 是右括号
                return false; // 没有左括号，或者左括号类型不对
            }
        }
        return st.isEmpty(); // 所有左括号必须匹配完毕
    }
}

```



method3:

用 if-else 代替 `mp`。

```java
class Solution {
    public boolean isValid(String s) {
        if (s.length() % 2 != 0) { // s 长度必须是偶数
            return false;
        }
        Deque<Character> st = new ArrayDeque<>();
        for (char c : s.toCharArray()) {
            if (c == '(') {
                st.push(')'); // 入栈对应的右括号
            } else if (c == '[') {
                st.push(']');
            } else if (c == '{') {
                st.push('}');
            } else if (st.isEmpty() || st.pop() != c) { // c 是右括号
                return false; // 没有左括号，或者左括号类型不对
            }
        }
        return st.isEmpty(); // 所有左括号必须匹配完毕
    }
}
```

复杂度分析
时间复杂度：O(n)，其中 n 是 s 的长度。
空间复杂度：O(n) 或 O(1)。如果能修改 s，那么直接把 s 当作栈，可以做到 O(1) 额外空间。

```java
class Solution {
    public boolean isValid(String s) {
        Deque<Character> stack = new ArrayDeque<>();
        int n = s.length();
        for (int i = 0; i < n; i++){
            char cur = s.charAt(i);
            if (cur == '('){
                stack.offerLast(cur);
                continue;
            }

            if (cur == '{'){
                stack.offerLast(cur);
                continue;
            }

            if (cur == '['){
                stack.offerLast(cur);
                continue;
            }

            if (!stack.isEmpty()){
                char curTop = stack.pollLast();
                if (cur == ')'){
                    if (curTop != '('){
                        return false;
                    }
                }else if (cur == ']'){
                    if (curTop != '['){
                        return false;
                    }
                }else if (cur == '}'){
                    if (curTop != '{'){
                        return false;
                    }
                }
            }else{
                return false;
            }
        }

        if (!stack.isEmpty()){
            return false;
        }

        return true;
    }
}

// TC: O(n)
// SC: O(n)
```



```java
class Solution {
    public boolean isValid(String s) {
        Deque<Character> stack = new ArrayDeque<Character>();
        char[] sArray = s.toCharArray();
        for (int i = 0; i < sArray.length; i++){
            if (sArray[i] == '('){
                stack.offerLast(')');
                continue;
            }

            if (sArray[i] == '{'){
                stack.offerLast('}');
                continue;
            }
            
            if (sArray[i] == '['){
                stack.offerLast(']');
                continue;
            }

            if (stack.isEmpty()){
                return false;
            }

            if (stack.pollLast() != sArray[i]){
                return false;
            }

    
        }

        return stack.isEmpty();
    }
}

// TC: O(n)
// SC: O(n)
/*

 [   { }   ]

 

 ^              }
 |             ]

 [:     
*/
```



```java
class Solution {
    public boolean isValid(String s) {
        Deque<Character> stack = new ArrayDeque<>();
        int n = s.length();
        if (n == 1){
            return false;
        }
        stack.offerLast(s.charAt(0));
        for (int i = 1; i < n; i++){
            char cur = s.charAt(i);
            if (cur == '(' || cur == '[' || cur == '{'){
                stack.offerLast(cur);
                continue;
            }

            if (stack.isEmpty()){
                return false;
            }
            char curStack = stack.pollLast();
            if (cur == ')'){
                if (curStack != '('){
                    return false;
                }
            }

            if (cur == ']'){
                if (curStack != '['){
                    return false;
                }
            }

            if (cur == '}'){
                if (curStack != '{'){
                    return false;
                }
            }

        }

        if (!stack.isEmpty()){
            return false;
        }

        return true;
    }
}

// TC: O(n)
// SC: O(n)
```

