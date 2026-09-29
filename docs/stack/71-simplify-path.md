---
tags:
    - String
    - Stack
    - Top Interviews
---

# [71. Simplify Path](https://leetcode.com/problems/simplify-path/)

You are given an *absolute* path for a Unix-style file system, which always begins with a slash `'/'`. Your task is to transform this absolute path into its **simplified canonical path**.

The *rules* of a Unix-style file system are as follows:

- A single period `'.'` represents the current directory.
- A double period `'..'` represents the previous/parent directory.
- Multiple consecutive slashes such as `'//'` and `'///'` are treated as a single slash `'/'`.
- Any sequence of periods that does **not match** the rules above should be treated as a **valid directory or** **file** **name**. For example, `'...' `and `'....'` are valid directory or file names.

The simplified canonical path should follow these *rules*:

- The path must start with a single slash `'/'`.
- Directories within the path must be separated by exactly one slash `'/'`.
- The path must not end with a slash `'/'`, unless it is the root directory.
- The path must not have any single or double periods (`'.'` and `'..'`) used to denote current or parent directories.

Return the **simplified canonical path**.

 

**Example 1:**

**Input:** path = "/home/"

**Output:** "/home"

**Explanation:**

The trailing slash should be removed.

**Example 2:**

**Input:** path = "/home//foo/"

**Output:** "/home/foo"

**Explanation:**

Multiple consecutive slashes are replaced by a single one.

**Example 3:**

**Input:** path = "/home/user/Documents/../Pictures"

**Output:** "/home/user/Pictures"

**Explanation:**

A double period `".."` refers to the directory up a level (the parent directory).

**Example 4:**

**Input:** path = "/../"

**Output:** "/"

**Explanation:**

Going one level up from the root directory is not possible.

**Example 5:**

**Input:** path = "/.../a/../b/c/../d/./"

**Output:** "/.../b/d"

**Explanation:**

`"..."` is a valid name for a directory in this problem.

 

**Constraints:**

- `1 <= path.length <= 3000`
- `path` consists of English letters, digits, period `'.'`, slash `'/'` or `'_'`.
- `path` is a valid absolute Unix path.



**Solution:**

题意
给你一组由 / 隔开的字符串（忽略空串和 .），请你从左到右遍历这些字符串，依次删除每个 .. 及其左侧的字符串（模拟返回上一级目录）。

思路
把 path 用 / 分割，得到一个字符串列表。

遍历字符串列表的同时，用栈维护遍历过的字符串：

如果当前字符串是空串或者 .，什么也不做（跳过）。
如果当前字符串不是 ..，那么把字符串入栈。
否则弹出栈顶字符串（前提是栈不为空），模拟返回上一级目录。
最后把栈中字符串用 / 拼接起来（最前面也要有个 /）。

```java
class Solution {
    public String simplifyPath(String path) {
        Deque<String> stack = new ArrayDeque<>();

        for (String s : path.split("/")){
            if (s.isEmpty() || s.equals(".")){
                continue;
            }

            if (!s.equals("..")){
                stack.offerLast(s);
            }else if (!stack.isEmpty()){
                stack.pollLast(); 
            }
        }

        return "/" + String.join("/", stack);
    }
}

// TC: O(n)
// SC: O(n)
```



```java
class Solution {
    public String simplifyPath(String path) {
        Deque<String> stack = new ArrayDeque<>();

        for (String p : path.split("/")){
            // home  foo <-

            if (p.isEmpty() || p.equals(".")){
                continue;
            }

            if (p.equals("..")){
                if (stack.isEmpty()){
                    continue;
                }else{
                    stack.pollLast();
                }
            }else{
                stack.offerLast(p);
            }

        }   


        // if (stack.isEmpty()){
        //     return "/";
        // }


        // StringBuilder sb = new StringBuilder();

        // while(!stack.isEmpty()){
        //     sb.append("/");
        //     sb.append(stack.pollFirst());

        // }
        // return sb.toString();

        return "/" + String.join("/", stack);
    }
}
```

