---
tags:
    - Two Pointers	
    - Design
    - Sorting
    - Heap (Priority Queue)
    - Data Stream
    - Top Interviews
---

# [295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value, and the median is the mean of the two middle values.

- For example, for `arr = [2,3,4]`, the median is `3`.
- For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.

Implement the MedianFinder class:

- `MedianFinder()` initializes the `MedianFinder` object.
- `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
- `double findMedian()` returns the median of all elements so far. Answers within `10-5` of the actual answer will be accepted.

**Example 1:**

```
Input
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
Output
[null, null, null, 1.5, null, 2.0]

Explanation
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0 
```

**Constraints:**

- `-105 <= num <= 105`
- There will be at least one element in the data structure before calling `findMedian`.
- At most `5 * 104` calls will be made to `addNum` and `findMedian`.

**Follow up:**

- If all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?
- If `99%` of all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?

**Solution:**

比如现在有 6 个数：1,5,6,2,3,4，要计算中位数，可以把这 6 个数从小到大排序，得到 1,2,3,4,5,6，中间 3 和 4 的平均值 3.5 就是中位数。

回顾一下百科中关于中位数的定义：中位数……可将数值集合划分为相等的两部分。

中位数把这 6 个数均分成了左右两部分，一边是 left=[1,2,3]，另一边是 right=[4,5,6]。我们要计算的中位数，就来自 left 中的最大值，以及 right 中的最小值。

随着 addNum 不断地添加数字，我们需要：

* 保证 left 的大小和 right 的大小尽量相等。规定：在有奇数个数时，left 比 right 多 1 个数。
* 保证 left 的所有元素都小于等于 right 的所有元素.

只要时时刻刻满足以上两个要求（满足中位数的定义），我们就可以用 left 中的最大值以及 right 中的最小值计算中位数。

分类讨论：

* 如果当前 left 的大小和 right 的大小相等：
  * 如果添加的数字 num 比较大，比如添加 7，那么把 7 加到 right 中。现在 left 比 right 少 1 个数，不符合前文的规定，所以必须把 right 的最小值从 right 中去掉，添加到 left 中。如此操作后，可以保证 left 的所有元素都小于等于 right 的所有元素。
  * 如果添加的数字 num 比较小，比如添加 0，那么把 0 加到 left 中。
  * 这两种情况可以合并：无论 num 是大是小，都可以先把 num 加到 right 中，然后把 right 的最小值从 right 中去掉，并添加到 left 中。
* 如果当前 left 比 right 多 1 个数：
  * 如果添加的数字 num 比较大，比如添加 7，那么把 7 加到 right 中。
  * 如果添加的数字 num 比较小，比如添加 0，那么把 0 加到 left 中。现在 left 比 right 多 2 个数，不符合前文的规定，所以必须把 left 的最大值从 left 中去掉，添加到 right 中。如此操作后，可以保证 left 的所有元素都小于等于 right 的所有元素。
  * 这两种情况可以合并：无论 num 是大是小，都可以先把 num 加到 left 中，然后把 left 的最大值从 left 中去掉，并添加到 right 中。

最后，我们需要什么样的数据结构？这个数据结构要能高效地执行如下操作：

* 添加元素。
* 找到最大（小）值。
* 删除最大（小）值。

这个数据结构是堆。

left 是最大堆，right 是最小堆。

* 如果当前有奇数个元素，中位数是 left 的堆顶。
* 如果当前有偶数个元素，中位数是 left 的堆顶和 right 的堆顶的平均值。



```java
class MedianFinder { // [2,3,4]
    PriorityQueue<Integer> left;// max
    PriorityQueue<Integer> right; // min

    public MedianFinder() {
        this.left = new PriorityQueue<>((a, b) -> b - a);
        this.right = new PriorityQueue<>();
    }
    
    public void addNum(int num) {
        if (left.size() == right.size()){
            right.offer(num);
            left.offer(right.poll());
        }else{
            left.offer(num);
            right.offer(left.poll());
        }
    }
    
    public double findMedian() {
        if (left.size() > right.size()){
            return left.peek();
        }

        return (left.peek() + right.peek())/2.0;
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */

// TC: 初始化和 findMedian 都是 O(1)，addNum 是 O(logq)，其中 q 是 addNum 的调用次数。每次操作堆需要 O(logq) 的时间。
// SC: 空间复杂度：O(q)。
```



```java
class MedianFinder {
    private PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>();
    private PriorityQueue<Integer> maxHeap = new PriorityQueue<Integer>(Collections.reverseOrder());
    private boolean even = true;

    public MedianFinder() {
        
    }
    
    public void addNum(int num) { // O(logn)
        if (even == true){
            maxHeap.offer(num);
            minHeap.offer(maxHeap.poll());
        }else{
            minHeap.offer(num);
            maxHeap.offer(minHeap.poll());
        }

        even = !even;
    }
    
    public double findMedian() { 
        if (even == true){
            return (minHeap.peek() + maxHeap.peek())/2.0;
        }else{
            return minHeap.peek();
        }
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */


// TC:O(logn)
// SC:O(n)
```





```java
class MedianFinder {
    PriorityQueue<Integer> firstPQ; //  -1  -3// max heap
    PriorityQueue<Integer> secondPQ; //  -2   -5 // minHeap  
    int len;
    public MedianFinder() {
        // 2   <-3        4  
        // -1     -2 
        // -3 
        // -5 , -1 -3, -2, 1

        firstPQ = new PriorityQueue<Integer>((a, b) -> (b - a));
        secondPQ = new PriorityQueue<Integer>();
        len = 0;
    }
    
    public void addNum(int num) {
        if (len % 2 == 0){
            firstPQ.offer(num);
            secondPQ.offer(firstPQ.poll());
            len++;
        }else{
            secondPQ.offer(num);
            firstPQ.offer(secondPQ.poll());
            len++;
        }
    }

    public double findMedian() {
        if (len % 2 == 0){
            return ((double) secondPQ.peek() + (double) firstPQ.peek())/2.0;
        }else{
            return (double) secondPQ.peek();
        }
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */

 // TC: O(nlogn)
 // SC: O(n)
```

