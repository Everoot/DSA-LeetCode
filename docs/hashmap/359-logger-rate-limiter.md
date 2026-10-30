---
tags:
    - Hash Table
    - Design
    - Data Stream
    - Top Interviews
---

# [359. Logger Rate Limiter](https://leetcode.com/problems/logger-rate-limiter/)

Design a logger system that receives a stream of messages along with their timestamps. Each **unique** message should only be printed **at most every 10 seconds** (i.e. a message printed at timestamp `t` will prevent other identical messages from being printed until timestamp `t + 10`).

All messages will come in chronological order. Several messages may arrive at the same timestamp.

Implement the `Logger` class:

- `Logger()` Initializes the `logger` object.
- `bool shouldPrintMessage(int timestamp, string message)` Returns `true` if the `message` should be printed in the given `timestamp`, otherwise returns `false`.

 

**Example 1:**

```
Input
["Logger", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage"]
[[], [1, "foo"], [2, "bar"], [3, "foo"], [8, "bar"], [10, "foo"], [11, "foo"]]
Output
[null, true, true, false, false, false, true]

Explanation
Logger logger = new Logger();
logger.shouldPrintMessage(1, "foo");  // return true, next allowed timestamp for "foo" is 1 + 10 = 11
logger.shouldPrintMessage(2, "bar");  // return true, next allowed timestamp for "bar" is 2 + 10 = 12
logger.shouldPrintMessage(3, "foo");  // 3 < 11, return false
logger.shouldPrintMessage(8, "bar");  // 8 < 12, return false
logger.shouldPrintMessage(10, "foo"); // 10 < 11, return false
logger.shouldPrintMessage(11, "foo"); // 11 >= 11, return true, next allowed timestamp for "foo" is 11 + 10 = 21
```

 

**Constraints:**

- `0 <= timestamp <= 109`
- Every `timestamp` will be passed in non-decreasing order (chronological order).
- `1 <= message.length <= 30`
- At most `104` calls will be made to `shouldPrintMessage`.



**Solution:**

```java
class Logger {
    Map<String, Integer> map;
    // int curTime;

    public Logger() {
        map = new HashMap<String, Integer>();
        // curTime = 0;
    }
    
    public boolean shouldPrintMessage(int timestamp, String message) {
        if (!map.containsKey(message)){
            map.put(message, timestamp + 10);
            return true;
        }else{
            int rightTime = map.get(message);
            if (timestamp >= rightTime){
                map.put(message, timestamp + 10);
                return true;
            }else{
                return false;
            }
        }
        
    }
}


// TC: O(1)
// SC: O(n)

/**
 * Your Logger object will be instantiated and called as such:
 * Logger obj = new Logger();
 * boolean param_1 = obj.shouldPrintMessage(timestamp,message);
 */
```



follow up question1: What if the map keeps growing over time and causes a memory issue?

add Queue

```java
import java.util.AbstractMap;
class Logger {
    Map<String, Integer> map;    
    Deque<Map.Entry<String, Integer>> queue;
    public Logger() {
        this.map = new HashMap<>();
        this.queue = new ArrayDeque<>();
    }
    
    public boolean shouldPrintMessage(int timestamp, String message) {
        // remove expired entries
        while(!queue.isEmpty() && timestamp >= queue.peek().getValue() + 10){
            Map.Entry<String, Integer> old = queue.pollFirst();

            String oldMessage = old.getKey();
            int oldTimestamp = old.getValue();

            if (map.containsKey(oldMessage) && map.get(oldMessage) == oldTimestamp){
                map.remove(oldMessage);
            }
            // if the oldmessage already update no need remove
        }
        if (!map.containsKey(message)){
            map.put(message, timestamp);
            queue.offerLast(new AbstractMap.SimpleEntry<>(message, timestamp));
            return true;
        }else{
            int pre = map.get(message);
            if (timestamp >= pre + 10){
                map.put(message, timestamp);
                queue.offerLast(new AbstractMap.SimpleEntry<String, Integer>(message, timestamp));
                return true;
            }else{
                return false;
            }
        }
    }
}
// TC: O(n)
// SC: O(n)
```



follow up question2: What if the timestamps do not arrive in chronological order?

```java
class Logger {
    Map<String, TreeSet<Integer>> map;
    public Logger() {
        this.map = new HashMap<>();
    }
    
    public boolean shouldPrintMessage(int timestamp, String message) {
        if (!map.containsKey(message)){
            TreeSet<Integer> set = new TreeSet<>();
            set.add(timestamp);
            map.put(message, set);

            return true;
        }

        TreeSet<Integer> set = map.get(message);

        Integer pre = set.floor(timestamp);
        Integer next = set.ceiling(timestamp);


        /*
        foo:10 foo:30

        Now: timestamp = 25

        floor(25) = 10
        25 - 10 = 15 看起来可以打印
        ceiling(25) = 30
        30 - 25 = 5 和 30 太近
        */
        // check previous timestamp
        if (pre != null && timestamp < pre + 10){
            return false;
        }

        // check next timestamp
        if (next != null && next < timestamp + 10){
            return false;
        }

        set.add(timestamp);
        return true;
    }
}

/**
 * Your Logger object will be instantiated and called as such:
 * Logger obj = new Logger();
 * boolean param_1 = obj.shouldPrintMessage(timestamp,message);
 */
// TC: O(log n)
// SC: O(n)
```



