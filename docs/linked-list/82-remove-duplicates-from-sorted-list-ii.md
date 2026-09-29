---
tags:
    - Linked List
    - Two Pointers
    - Top Interviews
---



# [82. Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

Given the `head` of a sorted linked list, *delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list*. Return *the linked list **sorted** as well*.

 

**Example 1:**

![img](./82. Remove Duplicates from Sorted List II/linkedlist1.jpg)

```
Input: head = [1,2,3,3,4,4,5]
Output: [1,2,5]
```

**Example 2:**

![img](./82. Remove Duplicates from Sorted List II/linkedlist2.jpg)

```
Input: head = [1,1,1,2,3]
Output: [2,3]
```

 

**Constraints:**

- The number of nodes in the list is in the range `[0, 300]`.
- `-100 <= Node.val <= 100`
- The list is guaranteed to be **sorted** in ascending order.



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
    public ListNode deleteDuplicates(ListNode head) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode cur = dummy;

        while(cur.next != null && cur.next.next != null){
            int val = cur.next.val;

            if (cur.next.next.val == val){
                while(cur.next != null && cur.next.val == val){
                    cur.next =cur.next.next;
                }
            }else{
                cur =cur.next;
            }
        }
        return dummy.next;
    }
}


// TC: O(n)
// SC: O(1)
```

![Screenshot 2025-09-09 at 10.28.04](./82. Remove Duplicates from Sorted List II/Screenshot 2025-09-09 at 10.28.04.png)

![Screenshot 2025-09-09 at 10.28.16](./82. Remove Duplicates from Sorted List II/Screenshot 2025-09-09 at 10.28.16.png)



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
    public ListNode deleteDuplicates(ListNode head) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode cur = dummy;

        while(cur.next != null && cur.next.next != null){
            int val = cur.next.next.val;

            if (cur.next.val == val){
                while(cur.next != null && cur.next.val == val){
                    cur.next = cur.next.next;
                }
            }else{
                cur = cur.next;
            }
        }

        return dummy.next;
    }
}
```

