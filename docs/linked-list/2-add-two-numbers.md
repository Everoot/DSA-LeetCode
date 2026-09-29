---
tags:
    - Linked List
    - Math
    - Recursion
    - Top Interviews
---

# [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

 

**Example 1:**

![img](./2 Add Two Numbers.assets/addtwonumber1.jpg)

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```

**Example 2:**

```
Input: l1 = [0], l2 = [0]
Output: [0]
```

**Example 3:**

```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

Solution:

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy;
        int carry = 0;

        while(l1 != null || l2 != null || carry != 0){
            int curSum = carry;
            if (l1 != null){
                curSum = curSum + l1.val;
                l1 = l1.next;
            }

            if (l2 != null){
                curSum = curSum + l2.val;
                l2 = l2.next;
            }

            carry = curSum / 10;
            cur.next = new ListNode(curSum % 10);
            cur = cur.next;


        }   

        return dummy.next;
    }
}

// TC: O(n)
// SC: O(n)
```



```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy; 
        int carry = 0;

        while (l1 != null || l2 != null || carry != 0){
            int x = (l1 != null) ? l1.val : 0;
            int y = (l2 != null) ? l2.val : 0;

            int sum = carry + x + y;
            carry = sum / 10;
            cur.next = new ListNode(sum % 10);
            cur = cur.next;
            if (l1 != null){
                l1 = l1.next;
            }

            if (l2 != null){
                l2 = l2.next;
            }
        }

        return dummy.next;
    }
}
// TC: O(n)
// SC: O(1)
```



```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy;

        int carry = 0;

        while(l1 != null && l2 != null){
            int curSum = l1.val + l2.val + carry;

            carry = curSum / 10;
            curSum = curSum % 10;

            cur.next = new ListNode(curSum);
            cur = cur.next;
            l1 = l1.next;
            l2 = l2.next;
        }

        while(l1 != null){
            int curSum = l1.val + carry;
            carry = curSum / 10;
            curSum = curSum % 10;
            cur.next = new ListNode(curSum);
            cur = cur.next;
            l1 = l1.next;

        }

        while(l2 != null){
            int curSum = l2.val + carry;
            carry = curSum / 10;
            curSum = curSum % 10;
            cur.next = new ListNode(curSum);
            cur = cur.next;
            l2 = l2.next;
        }

        if (carry != 0){
            cur.next = new ListNode(carry);
            cur = cur.next;
        }

        return dummy.next;

    }
}
// TC: O(n)
// SC: O(1)
```



```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;

        int carry = 0;

        while (l1 != null || l2 != null || carry ==1){
            int sum = 0;
            if (l1 != null){
                sum = sum +l1.val;
                l1 = l1.next;
            }

            if (l2 != null){
                sum = sum + l2.val;
                l2 = l2.next;
            }
            sum = sum + carry;
            carry = sum/10;
            ListNode node = new ListNode(sum % 10);
            curr.next = node;
            curr = curr.next;
        }
        return dummy.next;

        
    }
}

// TC(O(n))
// SC(O(1))
```



```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(-1);

        // 3 4 2 
        // 4 6 5

        int count = 0;
        long l1Val = 0;

        // [2,4,3]
        while(l1 != null){
            l1Val = l1Val +  (long) Math.pow(10, count) * l1.val;
            // 2   4   42 
            count++;
            l1 = l1.next;
        }

        count = 0;
        long l2Val = 0;
        while(l2 != null){
            l2Val = l2Val + (long) Math.pow(10, count) * l2.val;
            count++;
            l2 = l2.next;
        }

        long sum = l1Val + l2Val;
        ListNode cur = dummy;
        if (sum == 0){
            dummy.next = new ListNode(0);
            return dummy.next;
            
        }
        while(sum != 0){
            long curVal = sum % 10;
            cur.next = new ListNode((int) curVal);
            cur = cur.next;
            sum = sum / 10;
        }

        

        return dummy.next;


        
    }
}  

/*
Wrong Answer
1566 / 1569 testcases passed
submitted at Dec 26, 2024 16:38

Editorial
Input
l1 =
[1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1]
l2 =
[5,6,4]

Use Testcase
Output
[-3,-4,-3,-5,-7,-7,-4,-5,-8,-6,-3,0,-2,-7,-3,-3,-2,-2,-9]
Expected
[6,6,4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1]
This doesn't support visualization.

*/
```

由于 Java 的 `long` 类型无法处理超大数，可以使用 **字符串** 来保存数字，然后对字符串进行加法运算，避免溢出问题



````java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy;
        int carry = 0;
        while(l1 != null && l2 != null){
            int val_l1 = l1.val;
            int val_l2 = l2.val;
            int curSum = val_l1 + val_l2 + carry;
            carry = curSum / 10;
            curSum = curSum % 10;
            cur.next = new ListNode(curSum);
            cur = cur.next;
            l1 = l1.next;
            l2 = l2.next;
        }

        while(l1 != null){
            int val_l1 = l1.val;
            int curSum = l1.val + carry;
            carry = curSum / 10;
            curSum = curSum % 10;
            cur.next = new ListNode(curSum);
            cur = cur.next;
            l1 = l1.next;
        }

        while(l2 != null){
            int val_l2 = l2.val;
            int curSum = l2.val + carry;
            carry = curSum / 10;
            curSum = curSum % 10;
            cur.next = new ListNode(curSum);
            cur = cur.next;
            l2 = l2.next;
        }

        if (carry == 1){
            cur.next = new ListNode(carry);
        }

        return dummy.next;
    }
}

// TC: O(n)
// SC: O(n)
````





```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
       int carry = 0;
       ListNode dummy = new ListNode(-1);
       ListNode cur = dummy;

       while(l1 != null && l2 != null){
        int curL1 = l1.val;
        int curL2 = l2.val;
        int curSum = curL1 + curL2 + carry;
        cur.next = new ListNode(curSum % 10);
        carry = curSum / 10;
        cur = cur.next;
        l1 = l1.next;
        l2 = l2.next;
       } 

       while(l1 != null){
        int curSum = l1.val + carry;
        carry = curSum / 10;
        cur.next = new ListNode(curSum % 10);
        cur = cur.next;
        l1 = l1.next;
       }

       while(l2 != null){
        int curSum = l2.val + carry;
        carry = curSum / 10;
        cur.next = new ListNode(curSum % 10);
        cur = cur.next;
        l2 = l2.next;
       }

       if (carry != 0){
        cur.next = new ListNode(carry);
       }

       return dummy.next;
    }
}
```





