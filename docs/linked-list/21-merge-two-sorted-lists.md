---
tags:
    - Linked List
    - Recursion
    - Top Interview 150
---

# [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) ✅

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.

 

**Example 1:**

![img](./21. Merge Two Sorted Lists/merge_ex1.jpg)

```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

**Example 2:**

```
Input: list1 = [], list2 = []
Output: []
```

**Example 3:**

```
Input: list1 = [], list2 = [0]
Output: [0]
```

 

**Solution:**

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
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy;

        while(list1 != null && list2 != null){
            if (list1.val <= list2.val){
                cur.next = list1;
                cur = cur.next;
                list1 = list1.next;
            }else{
                cur.next = list2;
                cur = cur.next;
                list2 = list2.next;
            }
        }

        if (list1 != null){
            cur.next = list1;
        }else{
            cur.next = list2;
        }

        return dummy.next;
    }
}
// TC: O(n)
// SC: O(1) // O(n) 
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
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        if (list1 == null){
            return list2;
        }

        if (list2 == null){
            return list1;
        }

        ListNode head;
        if (list2.val < list1.val){
            head = list2;
            list2 = list2.next;
        }else{
            head = list1;
            list1 = list1.next;
        }

        ListNode cur = head;

        while(list1 != null && list2 != null){
            if (list1.val < list2.val){
                cur.next = list1;
                cur = cur.next;
                list1 = list1.next;
            }else{
                cur.next = list2;
                cur = cur.next;
                list2 = list2.next;
            }
        }

        if (list1 != null){
            cur.next = list1;
        }

        if (list2 != null){
            cur.next = list2;
        }

        return head;
        
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
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
       ListNode dummy = new ListNode(-1);

       ListNode cur = dummy;

       while(list1 != null && list2 != null){
        if (list1.val <= list2.val){
            cur.next = list1;
            list1= list1.next;
            cur = cur.next;
        }else{
            cur.next = list2;
            list2= list2.next;
            cur = cur.next;
        }
       }

       if (list1 != null){
        cur.next = list1;
       }

       if (list2 != null){
        cur.next = list2;
       }

       return dummy.next;
    }
}

// TC: O(n)
// SC: O(1)
```

