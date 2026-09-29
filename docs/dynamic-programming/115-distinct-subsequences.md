# [115. Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

Given two strings s and t, return *the number of distinct* ***subsequences\*** *of* s *which equals* t.

The test cases are generated so that the answer fits on a 32-bit signed integer.

 

**Example 1:**

```
Input: s = "rabbbit", t = "rabbit"
Output: 3
Explanation:
As shown below, there are 3 ways you can generate "rabbit" from s.
rabbbit
rabbbit
rabbbit
```

**Example 2:**

```
Input: s = "babgbag", t = "bag"
Output: 5
Explanation:
As shown below, there are 5 ways you can generate "bag" from s.
babgbag
babgbag
babgbag
babgbag
babgbag
```

 

**Constraints:**

- `1 <= s.length, t.length <= 1000`
- `s` and `t` consist of English letters.



**Solution:**

```java
class Solution {
    public int numDistinct(String s, String t) {
        int M = s.length();
        int N = t.length();

        int[] dp = new int[N];

        int prev = 1;

        // Iterate over the strings in reverse so as to
        // satisfy the way we've modeled our recursive solution
        for (int i = M - 1; i >= 0; i--) {
            // At each step we start with the last value in
            // the row which is always 1. Notice how we are
            // starting the loop from N - 1 instead of N like
            // in the previous solution.
            prev = 1;

            for (int j = N - 1; j >= 0; j--) {
                // Record the current value in this cell so that
                // we can use it to calculate the value of dp[j - 1]
                int old_dpj = dp[j];

                // If the characters match, we add the
                // result of the next recursion call (in this
                // case, the value of a cell in the dp table
                if (s.charAt(i) == t.charAt(j)) {
                    dp[j] += prev;
                }

                // Update the prev variable
                prev = old_dpj;
            }
        }

        return dp[0];
    }
}
```



