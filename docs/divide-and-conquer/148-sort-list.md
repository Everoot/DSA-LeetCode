# 148 Sort List

Given the `head` of a linked list, return *the list after sorting it in **ascending order***.

 

**Example 1:**

![img](./148 Sort List.assets/sort_list_1.jpg)

```
Input: head = [4,2,1,3]
Output: [1,2,3,4]
```

**Example 2:**

![img](./148 Sort List.assets/sort_list_2.jpg)

```
Input: head = [-1,5,3,4,0]
Output: [-1,0,3,4,5]
```

**Example 3:**

```
Input: head = []
Output: []
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
    public ListNode sortList(ListNode head) {
        // base case 
        if (head == null || head.next == null){
            return head;
        }

        ListNode mid = findmid(head);
        ListNode right = mid.next;
        mid.next = null;

        ListNode leftresult = sortList(head);
        ListNode rightresult = sortList(right);

        ListNode result = merge(leftresult, rightresult);
        return result;
    }

    private static ListNode findmid(ListNode head){
        ListNode fast = head;
        ListNode slow = head;
        while(fast.next != null && fast.next.next != null){
            fast = fast.next.next;
            slow = slow.next;
        }

        return slow;

        /* 
            [1 , 2 ,3 4]
                    f   f.n.n = null
                         5
                         f    f.n = null     
        */
    }

    private static ListNode merge(ListNode left, ListNode right){
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy;
        while (left != null && right != null){
            if (left.val <= right.val){
                cur.next = left;
                left = left.next;
                cur = cur.next;
            }else {
                cur.next = right;
                right = right.next;
                cur = cur.next;
            }
        }

        if (left != null){
            cur.next = left;
        }

        if (right != null){
            cur.next = right;
        }
        return dummy.next;
    }
}

// TC: Logn  * n = O(nlogn)
// SC: logn 



/*
     4  ->    2   ->  |   1      ->  3      
            /                   \
     [4 -> 2]                [ 1 -> 3]
      /  \                     /   \
    4     2                   1     3
    -----------------------------------
      \   /                    \   /
       [2 -> 4]                [1 -> 3]
                \            /
              [1 - 2 -> 3 -> 4]


*/
```





29. ==Merge Sort Linked List== (Lai)

Given a singly-linked list, where each node contains an integer value, sort it in ascending order. The merge sort algorithm should be used to solve this problem.

**Examples**

- null, is sorted to null
- 1 -> null, is sorted to 1 -> null
- 1 -> 2 -> 3 -> null, is sorted to 1 -> 2 -> 3 -> null
- 4 -> 2 -> 6 -> -3 -> 5 -> null, is sorted to -3 -> 2 -> 4 -> 5 -> 6

```java
package Class4LinkedList.Assignments;

import java.util.Arrays;

/*
29. Merge Sort Linked List
Given a singly-linked list, where each node contains an integer value,
sort it in ascending order. The merge sort algorithm should be used to solve this problem.

Examples
null, is sorted to null
1 -> null, is sorted to 1 -> null
1 -> 2 -> 3 -> null, is sorted to 1 -> 2 -> 3 -> null
4 -> 2 -> 6 -> -3 -> 5 -> null, is sorted to -3 -> 2 -> 4 -> 5 -> 6
 */
public class Ex9MergeSortLinkedList {
    public static class ListNode{
        public int value;
        public ListNode next;
        public ListNode(int value){
            this.value = value;
        }
    }

    public static void main(String[] args){
        int[] array3 = new int[]{4,2,6,3,5};
        ListNode node1 = buildListNodeFromArray(array3);
        //System.out.print(Arrays.toString(sortedArray(array3)));
        printListNode(mergeSort(node1));
    }

    public static ListNode mergeSort(ListNode head){
        if (head == null || head.next == null){
            return head;
        }
        // split the list into two halves
        ListNode middle = findMiddle(head);
        ListNode middleNext = middle.next;
        middle.next = null;
        // sort each half
        ListNode left = mergeSort(head);
        ListNode right = mergeSort(middleNext);
        // combine two halvees
        return merge(left, right);
    }

    public static ListNode merge(ListNode left, ListNode right){
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        while(left != null && right != null){
            if (left.value <= right.value){
                cur.next = left;
                left = left.next;
            } else {
                cur.next = right;
                right = right.next;
            }
            cur = cur.next;
        }
        if (left != null){
            cur.next = left;
            left = left.next;
        } else{
            cur.next = right;
            right = right.next;
        }
        return dummy.next;
    }

    public static ListNode findMiddle(ListNode head){
        if (head == null || head.next == null){
            return head;
        }
        ListNode fast = head;
        ListNode slow = head;
        while(fast.next != null && fast.next.next != null){
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }

    /*
    public static ListNode mergeSort(ListNode head) {
        // Write your solution here
        if (head == null || head.next == null){
            return head;
        }
        ListNode cur = head;
        int[] array = new int[count(head)];
        for (int i = 0; i < array.length; i++){
            array[i] = cur.value;
            cur = cur.next;
        }
        int[] array2 = sortedArray(array);
        return buildListNodeFromArray(array2);
    }



    public static int[] sortedArray(int[] array){
        if (array == null || array.length == 0){
            return array;
        }
        int j;
        for(int i = 0; i < array.length; i++){
            int min = array[i];
            for (j = i+1; j < array.length; j++){
                if (array[j] < min){
                    min = array[j];
                    swap(array,i,j);
                }
            }
        }
        return array;
    }

    public static int[] swap(int[] array, int i, int j){
        int rem = array[i];
        array[i] = array[j];
        array[j] = rem;
        return array;
    }

    public static int count(ListNode head){
        if (head == null){
            return 0;
        }
        int count = 0;
        while(head != null){
            count ++;
            head = head.next;
        }
        System.out.print(count);
        return count;

    }

     */

    public static void printListNode(ListNode head){
        if (head == null) {
            System.out.println("null");
        } else{
            ListNode cur = head;
            while(cur != null){
                System.out.print(cur.value + " -> ");
                cur = cur.next;
            }
            System.out.println("null");
        }
    }

    public static ListNode buildListNodeFromArray(int[] array){
        if(array == null || array.length == 0){
            return null;
        }
        ListNode head = new ListNode(array[0]);
        ListNode cur = head;
        for (int i = 1; i < array.length; i++){
            cur.next = new ListNode(array[i]);
            cur = cur.next;
        }
        return head;
    }
}
```

