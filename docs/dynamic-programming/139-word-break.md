---
tags:
    - Array
    - Hash Table
    - String
    - Dynamic Programming
    - Trie
    - Memoization
    - Top Interviews
---

# [139. Word Break](https://leetcode.com/problems/word-break/)

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

 

**Example 1:**

```
Input: s = "leetcode", wordDict = ["leet","code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

**Example 2:**

```
Input: s = "applepenapple", wordDict = ["apple","pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
Note that you are allowed to reuse a dictionary word.
```

**Example 3:**

```
Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
Output: false
```



**Solution:**

**Problem Description**

Split the string `s` into several segments such that **each segment exists in `wordDict`**.

Determine whether such a split is possible.

Note: You are allowed **not to split at all**. If `s` itself exists in `wordDict`, return `true`.



## 1. Identifying Subproblems

For example, let `s = "leetcode"`.
 We enumerate the **length of the last segment**:

- Length = 1 → substring `"e"` , If `"e"` is in `wordDict`, the problem becomes: Can `"leetcod"` be split into valid segments?
- Length = 2 → substring `"de"`, If `"de"` is in `wordDict`, the problem becomes: Can `"leetco"` be split into valid segments?
- Length = 3 → substring `"ode"` , If `"ode"` is in `wordDict`, the problem becomes: Can `"leetc"` be split into valid segments?
- Length = 4 → substring `"code"`, If `"code"` is in `wordDict`, the problem becomes: Can `"leet"` be split into valid segments?
- …

These are all **smaller subproblems with the same structure as the original problem**, so they can be solved using recursion.

⚠ **Important Note**

Although `wordDict` can contain up to **1000 strings**, there are **at most 20 distinct word lengths**.
 Therefore, we should **enumerate possible lengths**, rather than enumerating every word in `wordDict`.

------

**Additional Note**

Thinking **from right to left** makes it easier to convert the recursive solution into a dynamic programming (DP) recurrence.
 However, thinking **from left to right** is also valid.



## 2. State Definition and State Transition

Based on the discussion above, define the state:

**`dfs(i)`**: whether the prefix `s[:i]` (i.e., the substring from `s[0]` to `s[i−1]`) can be split into several segments such that **each segment exists in `wordDict`**.

------

### Enumerating the Last Segment of `s[:i]`

We enumerate the **length of the last segment** of `s[:i]`:

- **Length = 1**
   Substring: `s[i−1 : i]`
   If it exists in `wordDict`, the problem reduces to:
   Can `s[:i−1]` be split successfully?
   That is, check `dfs(i−1)`.
- **Length = 2**
   Substring: `s[i−2 : i]`
   If it exists in `wordDict`, check `dfs(i−2)`.
- **Length = 3**
   Substring: `s[i−3 : i]`
   If it exists in `wordDict`, check `dfs(i−3)`.
- …

Let `maxLen` be the **maximum length of words in `wordDict`**.
 We only need to enumerate lengths up to `maxLen`, because any longer substring **cannot** appear in `wordDict`.

### State Transition Formula

We enumerate:

```
j = i−1, i−2, i−3, …, max(i−maxLen, 0)
```

If there exists **any** `j` such that:

- `s[j:i]` is in `wordDict`, and
- `dfs(j) == true`

then:

```
dfs(i) = true
```

Otherwise:

```
dfs(i) = false
```

------

### Base Case (Recursion Boundary)

```
dfs(0) = true
```

An empty string means the entire string has been successfully split.

------

### Final Answer (Recursion Entry)

Let `n = s.length`.
 The final answer is:

```
dfs(n)
```

------

### Implementation Notes

- Convert `wordDict` into a **hash set** for O(1) substring lookup.
- This recursive formulation can be directly translated into **dynamic programming** (top-down with memoization or bottom-up DP).



### 3 Recursive Search

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
       int maxLen = 0;
       for (String word : wordDict){
        maxLen = Math.max(maxLen, word.length());
       }

       Set<String> words = new HashSet<>(wordDict);

       int n = s.length();

       return dfs(n, maxLen, s, words) == 1; 
    }

    private int dfs(int i, int maxLen, String s, Set<String> words){
        if (i == 0){
            return 1;
        }

        for (int j = i - 1; j >= Math.max(i - maxLen, 0); j--){
            if (words.contains(s.substring(j, i)) && dfs(j, maxLen, s, words) == 1){
                return 1;
            }
        }

        return 0;
    }
}
```

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        int maxLen = 0;
        for (String word : wordDict){
            maxLen = Math.max(maxLen, word.length());
        }

        Set<String> words = new HashSet<>(wordDict);

        int n = s.length();

        return dfs(n, maxLen, s, words);
    }

    private boolean dfs(int i, int maxLen, String s, Set<String> words){
        if (i == 0){
            return true;
        }

        for (int j = i - 1; j >= Math.max(i - maxLen, 0); j--){
            if (words.contains(s.substring(j, i)) && dfs(j, maxLen, s, words)){
                return true;
            }
        }
        return false;
    }
}
```



## 4. Recursive Search + Storing Return Values = Memoization

Considering that during the entire recursion process there are **a large number of repeated recursive calls** (with the same recursion parameters).
 Since the recursive function has **no side effects**, the result computed for the same input parameters will always be the same, no matter how many times it is computed.
 Therefore, we can use **memoized search** to optimize:

- If a state (i.e., recursion parameter) is encountered **for the first time**, before returning, store the state and its result in a `memo` array.
- If a state is **not encountered for the first time** (that is, the value stored in `memo` is not equal to the initial value), then we can directly return the result stored in `memo`.

------

⚠ **Note**:
 The initial value of the `memo` array **must not be equal to any value that needs to be memoized**!

For example, if the initial value is set to `0` (representing `false`), and the value of `dfs(i)` to be memoized can also be `0` (representing `false`), then it is impossible to determine whether `0` means:

- the state is encountered for the first time, or
- the state has been encountered before

which would cause memoization to fail.

Therefore, the initial value is generally set to `-1`.

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
       int maxLen = 0;
       for (String word : wordDict){
        maxLen = Math.max(maxLen, word.length());
       }

       Set<String> words = new HashSet<>(wordDict);

       int n = s.length();
       int[] memo = new int[n + 1];

       Arrays.fill(memo, -1); 

       return dfs(n, maxLen, s, words, memo) == 1; 
    }

    private int dfs(int i, int maxLen, String s, Set<String> words, int[] memo){
        if (i == 0){
            return 1;
        }

        if (memo[i] != -1){
            return memo[i];
        }

        for (int j = i - 1; j >= Math.max(i - maxLen, 0); j--){
            if (words.contains(s.substring(j, i)) && dfs(j, maxLen, s, words, memo) == 1){
                return memo[i] = 1;
            }
        }

        return memo[i] = 0;
    }
}

// TC: O(nL^2)
// SC: O(n)
```

* 时间复杂度: $O(mL+nL^2)$, 其中$m$是wordDict的长度, $L \leq 20$ 是wordDict中字符串的最长长度, n是s的长度. 创建哈希集合需要$O(mL)$的时间. 由于每个状态只会计算一次, 动态规划的时间复杂度= 状态个数 x 单个状态的计算时间. 本地状态个数等于O(n), 单个状态的计算时间为O(L^2) (注意判断子串是否在哈希集合中需要O)(L)的时间), 所以记忆化搜索的时间复杂度为O(nL^2)
* 空间复杂度: $O(mL+n)$. 哈希集合需要$O(mL)$的空间. 记忆化搜索需要$O(n)$的空间.

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        int maxLen = 0;
        for (String word : wordDict){
            maxLen = Math.max(maxLen, word.length());
        }

        Set<String> words = new HashSet<>(wordDict);

        int n = s.length();
        Boolean[] memo = new Boolean[n + 1];
        return dfs(n, s, words, memo);
    }

    private boolean dfs(int i, String s, Set<String> words, Boolean[] memo){
        if (i == 0){
            return true;
        }

        if (memo[i] != null){
            return memo[i];
        }

        for (int j = i - 1; j >= 0; j--){
            if (words.contains(s.substring(j, i)) && dfs(j, s, words, memo)){
                return memo[j] = true;
            }
        }
        return memo[i] = false;
    }
}
// TC: O(n * m * n)
// SC: O(n)
```



```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        int maxLen = 0;
        for (String word : wordDict){
            maxLen = Math.max(maxLen, word.length());
        }

        Set<String> words = new HashSet<>(wordDict);

        int n = s.length();
        int[] memo = new int[n + 1];
        Arrays.fill(memo, -1);
        return dfs(n, maxLen, s, words, memo);

    }

    private boolean dfs(int i, int maxLen, String s, Set<String> words, int[] memo){
        if (i == 0){
            return true;
        }

        if (memo[i] != -1){
            if (memo[i] == 1){
                return true;
            }else{
                return false;
            }
        }

        for (int j = i - 1; j >= Math.max(i - maxLen, 0); j--){
            if (words.contains(s.substring(j, i)) && dfs(j, maxLen, s, words, memo)){
                memo[i] = 1;
                return true;
            }
        }

        memo[i] = 0;
        return false;
    }
}
```



```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        int n = s.length();

        Set<String> set = new HashSet<>(wordDict); // m 
        // k as the average length of the words in k
        int[] memo = new int[n + 1]; // // O(n)
        Arrays.fill(memo, -1); // 1 = true 0 = false
        return dfs(n, s, set, memo);

    }

    private boolean dfs(int index, String s, Set<String> set, int[] memo){
        if (index == 0){
            return true;
        }

        if (memo[index] != -1){
            if (memo[index] == 1){
                return true;
            }else{
                return false;
            }
        }

        for (int j = index - 1; j >= 0; j--){ // k
            String cur = s.substring(j, index); /// [j, i - 1]
            if (set.contains(cur) && dfs(j, s, set, memo) == true){
                memo[index] = 1;
                return true;
            }
        }

        memo[index] = 0;
        return false;
    }
}

// TC: O(n * k * m)  < O(n^2)
// SC: O(n)

/*

 how can we split word, make sure the split word in the wordDict 
 if possible then return true;

 otherwise return false;
                       012345678
   i=n -1              leetcode
                           j   i               -> substring (j,i) -> find wether in the wordDict
                       / \    \   
        for (j)       i-1 i-2 i-3   
                               / \\ \    i= i -3           i = 0 -> ""            apple
                             
*/
```



### 5.  One-to-One Translation into an Iterative (DP) Approach

We can remove the “recursion” part from the recursive solution and keep only the “aggregation” part, computing the result bottom-up.

Specifically, the definition of `f[i]` is the same as the definition of `dfs(i)`: it indicates whether the prefix `s[:i]` (that is, `s[0]` to `s[i-1]`) can be segmented into several parts such that each part exists in `wordDict`.

Similarly, we enumerate
 `j = i − 1, i − 2, i − 3, …, max(i − maxLen, 0)`.
 If there exists any `j` such that `s[j:i]` is in `wordDict` **and** `f[j] = true`, then `f[i]` is `true`.

The initial value is `f[0] = true`, which is translated from the recursive base case `dfs(0) = true`.

The final answer is `f[n]`, translated from the recursive entry `dfs(n)`.

------

### Q&A

**Q:** Can we enumerate `words` in the outer loop and lengths in the inner loop, similar to a complete knapsack approach?

**A:** No. In a complete knapsack problem, once you repeatedly choose the same item, you eventually stop choosing it and move on. However, in this problem, words can be chosen alternately.
 For example, if `s` has an `ABA` pattern, a complete knapsack–style approach can only enumerate continuous combinations like `AAB` or `ABB`, but it cannot enumerate a pattern like `ABA`.

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
       int maxLen = 0;
       for (String word : wordDict){
        maxLen = Math.max(maxLen, word.length());
       }

       Set<String> words = new HashSet<>(wordDict);

       int n = s.length();
       int[] memo = new int[n + 1];

       Arrays.fill(memo, -1); 

       memo[0] = 1;
       for (int i = 1; i <= n; i++){
        for (int j = i - 1; j >= Math.max(i - maxLen, 0); j--){
            if (memo[j] == 1 && words.contains(s.substring(j, i))){
                memo[i] = 1;
                break;
            }
        }
       }

       return memo[n] == 1;

    //    return dfs(n, maxLen, s, words, memo) == 1; 
    }

    // private int dfs(int i, int maxLen, String s, Set<String> words, int[] memo){
    //     if (i == 0){
    //         return 1;
    //     }

    //     if (memo[i] != -1){
    //         return memo[i];
    //     }

    //     for (int j = i - 1; j >= Math.max(i - maxLen, 0); j--){
    //         if (words.contains(s.substring(j, i)) && dfs(j, maxLen, s, words, memo) == 1){
    //             return memo[i] = 1;
    //         }
    //     }

    //     return memo[i] = 0;
    // }
}

// TC: O(mL + nL^2)
// SC: O(mL + n)
```



```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        // leet code
        // i 
        // substring  (i, j ) wordDict -> true

        int n = s.length();
        Map<Integer, Integer> map = new HashMap<>();

        boolean[] dp = new boolean[n];

        // dp[i][j] means -> exit in the wordDict

        Trie trie = new Trie();
        for (String w : wordDict){
            trie.insert(w);
        }


        for (int i = 0; i < n; i++){
            if (i == 0 || dp[i - 1] == true){
                TrieNode node = trie.root;
                for (int j = i; j < n; j++){
                    char ele = s.charAt(j);
                    if (!node.children.containsKey(ele)){
                        break;
                    }

                    node = node.children.get(ele);
                    if (node.isWord){
                        dp[j] = true;
                    }
                }
            }


        }

        return dp[n - 1];

    
    }

    class TrieNode{
        Map<Character, TrieNode> children = new HashMap<>();
        boolean isWord = false;
    } 


    class Trie{
        public TrieNode root;

        Trie(){
            root = new TrieNode();
        }

        public void insert(String word){
            TrieNode cur = root;
            for (char ch : word.toCharArray()){
                if (cur.children.containsKey(ch)){
                    cur = cur.children.get(ch);
                }else{
                    cur.children.put(ch, new TrieNode());
                    cur = cur.children.get(ch);
                }
            }
            cur.isWord = true;
        }

        public boolean isPrefix(String word){
            TrieNode cur = root;
            for (char ch : word.toCharArray()){
                if (!cur.children.containsKey(ch)){
                    return false;
                }
            }
            return true;
        }

        public boolean isWord(String word){
            TrieNode cur = root;
            for (char ch : word.toCharArray()){
                if (!cur.children.containsKey(ch)){
                    return false;
                }
            }
            return cur.isWord;
        }
    }
}
```



```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
       // Assume input is not ""
       // M[i] represents the substring[0, i);
       // M[0] represents "" , M[1] represent s[0]; M[2] represent s[0-2]
       boolean[] M = new boolean[s.length() + 1];
       // That's why we need +1 here
       M[0] = true;
       // 0 represent ""
       for (int i = 1; i <= s.length(); i++){
           // i letter // linear scan 回头看
           for (int j = 0; j < i; j++){
               // first j letter as 左大段 // 这个for loop 在切
               if (M[j] && wordDict.contains(s.substring(j, i))){
                // cut left    && cut right
                // substring begins at index i and extends to index i-1
                   M[i] = true;
                   break;
               }

           }
       }
       return M[s.length()];

    }
}
// time =  O(n^3) because of the string.substring API and the hashset.contains API.
// Space = O(n) or O(n^3) if you cannot assume GC happens immediatel

// Off by 1
// M[i] 里的i 代表的含义不是index, 代表的是长度
// M[i] 长度为i的string 能不能被切分开每一段都在dict里
```



```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        boolean[] dp = new boolean[s.length() + 1];
        // dp[i] means from s [0, s) whether is word in the wordDict
        // dp[0] ""

        dp[0] = true; // ""


        for (int i = 1; i < s.length() + 1; i++){
            for (int j = 0; j < i; j++){
                if (dp[j] == true && wordDict.contains(s.substring(j, i))){
                    dp[i] = true; 
                    break;
                }
            }
        }

        return dp[s.length()];


    }
}

/*
    leetcode 
   TFFFT
       j
 */

 // TC: O(n^2)
 // SC: O(n)

```

