---
tags:
    - Math
    - String
    - Bit Manipulation
    - Simulation
    - Top Interviews
---

# [67. Add Binary](https://leetcode.com/problems/add-binary/)

Given two binary strings `a` and `b`, return *their sum as a binary string*.

**Example 1:**

```
Input: a = "11", b = "1"
Output: "100"
```

**Example 2:**

```
Input: a = "1010", b = "1011"
Output: "10101"
```

 

**Constraints:**

- `1 <= a.length, b.length <= 104`
- `a` and `b` consist only of `'0'` or `'1'` characters.
- Each string does not contain leading zeros except for the zero itself.



**Solution:**

```java
class Solution {
    public String addBinary(String a, String b) {
        int n = a.length();
        int m = b.length();
        int i = a.length() - 1;
        int j = b.length() - 1;
        int carry = 0;
        StringBuilder sb = new StringBuilder();
        while(i >= 0 || j >= 0 || carry == 1){
            int digit = 0;
            if (i >= 0){
                digit = a.charAt(i) - '0';
            }

            int digit2 = 0;
            if (j >= 0){
                digit2 = b.charAt(j) - '0';
            }

            int sum = digit + digit2 + carry;

            if (sum % 2 == 0){
                sb.append('0');
            }else{
                sb.append('1');
            }

            if (sum >= 2){ // it will have 3 , current poisiton 1 +1 and carry = 1 
                carry = 1;  // which should use >= 
            }else{
                carry = 0;
            }

            i--;
            j--;

        }

        return sb.reverse().toString();
    }
}

// TC: O(n)
// SC: O(1)
```



