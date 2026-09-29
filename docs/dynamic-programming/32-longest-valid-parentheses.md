---
tags:
    - String
    - Dynamic Programming
    - Stack
    - Top Interviews
---

# [32. Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)

Given a string containing just the characters `'('` and `')'`, return *the length of the longest valid (well-formed) parentheses* *substring*.

 

**Example 1:**

```
Input: s = "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()".
```

**Example 2:**

```
Input: s = ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()".
```

**Example 3:**

```
12Input: s = ""
Output: 0
```

 

**Constraints:**

- `0 <= s.length <= 3 * 104`
- `s[i]` is `'('`, or `')'`.



**Solution:**

```java
class Solution {
    public int longestValidParentheses(String s) {

        Deque<Integer> stack = new ArrayDeque<>();
        stack.offerLast(-1);
        int result = 0;
        for (int i = 0; i < s.length(); i++){
            if (s.charAt(i) == '('){
                stack.offerLast(i);
            }else if (stack.size() > 1){
                // stacktop is boom
                stack.pollLast();
                result = Math.max(result, i - stack.peekLast());
            }else {
                if (stack.isEmpty()){
                    stack.offerLast(i);
                }else{
                    stack.pollLast();
                    stack.offerLast(i);
                }

            }
        }

        return result;
    }
}
// TC: O(n)
// SC: O(n)
```

