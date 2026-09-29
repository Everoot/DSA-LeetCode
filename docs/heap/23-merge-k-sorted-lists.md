---
tags:
    - Linked List
    - Divide and Conquer
    - Heap (Priority Queue)
    - Merge Sort
---

# [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

**Example 1:**

```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation:23. Merge k Sorted Lists The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```

**Example 2:**

```
Input: lists = []
Output: []
```

**Example 3:**

```
Input: lists = [[]]
Output: []
```

  

**Solution:**

### Method 1:  minHeap

The first node after merging, $first$, must be the head node of one of the linked lists (since each list is sorted in ascending order).

The second node after merging could either be the head node of another linked list or the next node of $first$.

For example, given three linked lists:

`1 -> 2 -> 5`, `3 -> 4 -> 6`, and `4 -> 5 -> 6`, 

after finding the first node `1`, the second node is not the head of another list but rather the next node of `1`, which is `2`.

Following this logic, each time we find the node with the smallest value, sya x, we add x.next to the set of "possible smallest nodes".

Therefore, we need a data structure that supports the following operations:

* Find and remove the smallest node from the structure.
* Insert a new node.

This can be implemented using a min-heap. Initially, we push all the head nodes of the linked lists into the heap. Then, we repeatedly pop the smallest node `x` from the heap. If `x.next` is not null, we push it into the heap. We continue this process until the heap becomes empty. By connecting the popped nodes in order, we obtain the merged linked list.

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
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy;
        if (lists == null || lists.length == 0){
            return dummy.next;
        }
        PriorityQueue<ListNode> pq = new PriorityQueue<ListNode>((a, b) -> (a.val - b.val));

        for (int i = 0; i < lists.length; i++){
            if (lists[i] != null){            
                pq.offer(lists[i]);
            }

        }

        while(!pq.isEmpty()){
            ListNode pqCur = pq.poll();

            cur.next = pqCur;
            cur = pqCur;
            if (pqCur.next != null){
                pq.offer(pqCur.next);
            }
        }

        return dummy.next;


    }
}

// TC: O(nlogn)
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
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode dummy = new ListNode(-1);

        PriorityQueue<ListNode> pq = new PriorityQueue<ListNode>((a,b) -> Integer.compare(a.val, b.val));

        for (int i = 0; i < lists.length; i++){
            ListNode cur = lists[i];
            if (cur != null){
                pq.offer(cur);
            }
        }

        ListNode curr = dummy;

        while(!pq.isEmpty()){
            curr.next = pq.poll();
            if (curr.next.next != null){
                pq.offer(curr.next.next);
            }
            curr = curr.next;
        }

        return dummy.next;
    }
}
// TC: O(n)
// SC: O(n)
```



直接条件反射了

看到 K ... -> priorityQueue 

思路对, 但经常@Override 格式啥的 记不住

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
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0){
            return null;
        }

        PriorityQueue<ListNode> minHeap = new PriorityQueue<ListNode>(new Comparator<ListNode>(){
            @Override
            public int compare(ListNode one, ListNode two){
                if (one.val < two.val){
                    return -1;
                }else if (one.val == two.val){
                    return 0;
                }else{
                    return 1;
                }
            }
        });

        for (int i = 0; i < lists.length; i++){
            if (lists[i] != null){
                minHeap.offer(lists[i]);
            }

        }

        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy;
        while(!minHeap.isEmpty()){
            cur.next = minHeap.poll();
            if (cur.next.next != null){
                minHeap.offer(cur.next.next);
            }
            cur = cur.next;
        }

        return dummy.next;
    }
}
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
    public ListNode mergeKLists(ListNode[] lists) {
        PriorityQueue<ListNode> minHeap = new PriorityQueue<ListNode>(11, new MyComparator());


        for(ListNode node : lists){
            if (node != null){
                minHeap.offer(node);
            }
        }

                
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;

        while(!minHeap.isEmpty()){
            cur.next = minHeap.poll();
            if (cur.next.next != null){
                minHeap.offer(cur.next.next);
            }

            cur = cur.next;
        }

        return dummy.next;
    }

    static class MyComparator implements Comparator<ListNode> {
        public int compare(ListNode o1, ListNode o2){
            if (o1.val == o2.val){
                return 0;
            }else{
                if (o1.val < o2.val){
                    return -1;
                }else{
                    return 1;
                }
            }
        }
    }
}

// TC: O(nlogk + n) = O(nlogk)
// SC: O(k)
```

