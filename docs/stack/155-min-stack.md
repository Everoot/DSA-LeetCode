---
tags:
    - Stack
    - Design
    - Top Interviews
---

# [155. Min Stack](https://leetcode.com/problems/min-stack/)

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:

- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element `val` onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

 

**Example 1:**

```
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
```



**Solution:**

引入
给你一个数组 nums，如何计算每个前缀的最小值？

定义 preMin[i] 表示 nums[0] 到 nums[i] 的最小值。

这可以从左到右计算：

preMin[0]=nums[0]。
preMin[1]=min(nums[0],nums[1])。
preMin[2]=min(nums[0],nums[1],nums[2])=min(preMin[1],nums[2])。
preMin[3]=min(nums[0],nums[1],nums[2],nums[3])=min(preMin[2],nums[3])。
……
一般地，我们有

preMin[i]=min(preMin[i−1],nums[i])
回到本题
把 nums 视作栈，本题相当于在 nums 的末尾动态地添加/删除元素。

栈中除了保存添加的元素，还保存前缀最小值。（栈中保存的是 pair）
添加元素：设当前栈的大小是 n。添加元素 val 后，额外维护 preMin[n]=min(preMin[n−1],val)，其中 preMin[n−1] 是添加 val 之前，栈顶保存的前缀最小值。
删除元素：弹出栈顶即可。
细节
一开始栈为空（n=0），添加 val 时，我们没有对应的 preMin[n−1]。需要特判栈为空的情况吗？

不需要。初始化的时候，在栈底加一个 ∞ 哨兵，作为 preMin[−1]。

注意题目保证 pop,top,getMin 都是在非空栈上操作的。

```java
class MinStack {
    // 注意不要使用Stack类, 因为它继承自Vector,是同步的, 会导致一些性能问题.
    private final Deque<int[]> stack;
    public MinStack() {
       this.stack = new ArrayDeque<>();
       // 添加栈底哨兵 Integer.MAX_VALUE;
       // 这里的0写成任意数都可以, 反正用不到
       stack.offerLast(new int[]{0, Integer.MAX_VALUE});        
    }
    
    public void push(int val) {
        stack.offerLast(new int[]{val, Math.min(getMin(), val)});
    }
    
    public void pop() {
        stack.pollLast();
    }
    
    public int top() {
       return stack.peekLast()[0]; 
    }
    
    public int getMin() {
        return stack.peekLast()[1]; 
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(val);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */

```

复杂度分析
时间复杂度：所有操作均为 O(1)。
空间复杂度：O(q)。其中 q 是 push 调用的次数。最坏情况下，只有 push 操作，需要 O(q) 的空间保存元素。

变形题
改成队列（queue），getMin 返回队列中的最小元素。
改成双端队列（deque），getMin 返回双端队列中的最小元素。
解答：[Tutorial] Minimum Deque。





同压同弹空间优化:

```java
class MinStack {

    Deque<Integer> s1;
    Deque<Integer> s2;
    int min = Integer.MAX_VALUE;

    /*  
         5 3 2 3 
        min =  2          
        s1 (push) = [  5  3   
        s2 (min) = [  5 3 
        min = Integer.MAX_VALUE;
    */

    public MinStack() {
        s1 = new ArrayDeque<Integer>();
        s2 = new ArrayDeque<Integer>();
        
    }
    
    public void push(int val) {
        s1.offerLast(val);
        if (min >= val){
            min = val;
            s2.offerLast(min);
        }
    }
    
    public void pop() {
        int j = s1.pollLast();

        if (j == min){
            s2.pollLast();
        }

        if (s2.isEmpty()){
            min = Integer.MAX_VALUE;
        }else{
            min = s2.peekLast();

        }
    }
    
    public int top() {
        return s1.peekLast();
    }
    
    public int getMin() {
        return min;
    }
}
// TC: O(1)
// SC: O(n)

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(val);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```



同压同弹:

```java
class MinStack {

    Deque<Integer> s1;
    Deque<Integer> s2;
    int min = Integer.MAX_VALUE;

    /*  
         5 1 2 3 
        min =  1          
        s1 (push) = [  5 1 
        s2 (min) = [  5 1 
        min = Integer.MAX_VALUE;
    */

    public MinStack() {
        s1 = new ArrayDeque<Integer>();
        s2 = new ArrayDeque<Integer>();
        
    }
    
    public void push(int val) {
        s1.offerLast(val);
        if (min >= val){
            min = val;
        }

        s2.offerLast(min);
    }
    
    public void pop() {
        s1.pollLast();
        s2.pollLast();
        if (s2.isEmpty()){
            min = Integer.MAX_VALUE;
        }else{
            min = s2.peekLast();

        }
    }
    
    public int top() {
        return s1.peekLast();
    }
    
    public int getMin() {
        return min;
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(val);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```



这种写法时间复杂度太大了

```java
class MinStack {
    Deque<Integer> s1 = new ArrayDeque<Integer>();
    Deque<Integer> s2 = new ArrayDeque<Integer>();
    int min = Integer.MAX_VALUE;
    int max = Integer.MIN_VALUE;

    /*
        min stack
        s1 [ 2 3 4 2 3     
        s2 [    
    */
    public MinStack() {
        
    }
    
    public void push(int val) {
        s1.push(val);
        if (val < min){
           min = val; 
        }

        if (val > max){
            max = val;
        }
    }
    
    public void pop() {
        s1.pop();
        min = Integer.MAX_VALUE;
        max = Integer.MIN_VALUE;
        while(!s1.isEmpty()){
            int val = s1.pop();
            s2.push(val);
            if (val < min ){
                min = val;
            }

            if (val > max){
                max = val;
            }
        }

        while(!s2.isEmpty()){
            s1.push(s2.pop());
        }

    }
    
    public int top() {
        return s1.peek();
    }
    
    public int getMin() {
        return min;
    }
}
// -2 0 -1 
/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(val);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.
```





#### How to implement the min() function when using stack with time complexity O(1) for each operation;

Stack APIs: push(), pop(), top(),                  min()



How to use this data structures?

```java
MinStack s = new MinStack(); // stack: [3 5 -1        global min = -1
s.push(3);
s.min() // ->3
s.push(5);
s.min() // ->3
s.push(-1);
s.min(); // -> -1
s.pop();
s.min(); // -> -1
```

所以一个stack不够



**Method 1: keep the adding & removing action in synchronization with each other min() returns the stacks.top() value as the result**

Stack1 (input) [ 

Stack2 (min) [  



1. 来个了个3

   Stack1 (input) [ 3

   同压

   Stack2 (min) [  3

2. 再来个5

   Stack1 (input) [ 3 5

   同压

   Stack2 (min) [  3 3

3. 再来个-1

   Stack1 (input) [ 3 5 -1

   同压

   Stack2 (min) [  3 3 -1

同弹，有一天我想从input里弹掉一个

1. 弹-1

   Stack1 (input) [ 3 5  

   同弹

   Stack2 (min) [  3 3 



Stack1: is used to store input elements

Stack2: is used to store the min element so far in Stack1

俩stack同压同弹

Insight: try to make the elements in Stack2 a descending (with duplicate) order.

time: O(1)

space: O(n)





#### 举一反二： How to optimize the space usage of stack2?

Stack1 (input) [ 3 5 6 7 2 3 2

Stack2 (min) [ 3 3 3 3

没必要压这么多3333



push: 

case 1 when the value is greater than global min (top of stacks2): we don't care

case 2 when the value is less than global min, push to stack 2 as well



how about when equal to?



Correction:

case2: when the value is less than OR equals to global min, push to stack2 as well



pop:

case1 when the popped value is grater than global min(top of stack2): we don't care 

case2: when the popped value is less than global min, pop from stack2



Wait, is it possible we have a popped value that is less than global min?



Correction:

case2: when the popped value equals to global min, pop from stack2



You will still keep multiple copies of some numbers (if equal to global min) in stack2.

We can do even better!



Time = O(1)

Worst Space = O(n)



### 举一反三: How to optimize the space usage of stack2 if there are a lot of duplicate numbers?

Insight: try to make the elements in Stacks2 a descending (without duplicate) order.

size            		1      2      3       4       5     6        7     8      9        10      11     12

Stack1(input) [ 3      3       3      3       5     3        2      2      2        -1       -1     -1

Stack2(min)    [3       2       -1

Stack3(size)    [1      7        10



从两个方面考虑:

1. 重复性
2. 增减性

需要解决的根本问题是: 目前stack1 pop的元素和stack2栈顶相等的时候, 它到底是不是最后一个和它相等的(当它进来的时候是不是first occurrence), 如果是, 也要从stack2清理掉.

push:

case1

when the value is greater than OR equals to global min: 3 -> 5 OR 3 -> 3, we don't care

case2

when the value is less than global min: 3->3, that means we have a new global min, then we insert it to stack2 and insert the current size of stack1 to stack3.

pop:

case1 when the popped value is greater than global min: we don't care

case2 when the popped value equals to global min: check if the size after pop to decide if we are popping the red element (first occurrence), if yes, then pop from stack 2/3, if not, keep in track 2/3.



Stack1: is used to store input elements

Stack2: is used to store the min element so far in Stack1 (as its top element)

Stack3: the size of Stack1 when this element is added to Stack2



 OR we can use a pair to replace both stack2 and stack3:

```java
class MyPair{
  int globalMin;
  int sizeOfFirstOccurrence;
  MyPair(int m, int s){
    globalMin = m;
    sizeOfFirstOccurrence = s;
  }
}
```





In interviews: you can probably use int[2] with comments! Ask your interviewer.

size 					 1	2	3	4	5	6	7	8	9	10	11	12

Stack1(input)[    3   3     3    3     5	3	2   2 	2    -1      -1     -1     333 55 3 77 -4 33 2 5

Stack2(min/size)[<3,1>, <2, 7> <-1, 10>....]













