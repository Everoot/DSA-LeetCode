---
tags:
    - Hash Table
    - String
    - Binary Search
    - Design
    - Amazon
---

# [981. Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/)

Design a time-based key-value data structure that can store multiple values for the same key at different time stamps and retrieve the key's value at a certain timestamp.

Implement the `TimeMap` class:

- `TimeMap()` Initializes the object of the data structure.
- `void set(String key, String value, int timestamp)` Stores the key `key` with the value `value` at the given time `timestamp`.
- `String get(String key, int timestamp)` Returns a value such that `set` was called previously, with `timestamp_prev <= timestamp`. If there are multiple such values, it returns the value associated with the largest `timestamp_prev`. If there are no values, it returns `""`.

 

**Example 1:**

```
Input
["TimeMap", "set", "get", "get", "set", "get", "get"]
[[], ["foo", "bar", 1], ["foo", 1], ["foo", 3], ["foo", "bar2", 4], ["foo", 4], ["foo", 5]]
Output
[null, null, "bar", "bar", null, "bar2", "bar2"]

Explanation
TimeMap timeMap = new TimeMap();
timeMap.set("foo", "bar", 1);  // store the key "foo" and value "bar" along with timestamp = 1.
timeMap.get("foo", 1);         // return "bar"
timeMap.get("foo", 3);         // return "bar", since there is no value corresponding to foo at timestamp 3 and timestamp 2, then the only value is at timestamp 1 is "bar".
timeMap.set("foo", "bar2", 4); // store the key "foo" and value "bar2" along with timestamp = 4.
timeMap.get("foo", 4);         // return "bar2"
timeMap.get("foo", 5);         // return "bar2"
```

 

**Constraints:**

- `1 <= key.length, value.length <= 100`
- `key` and `value` consist of lowercase English letters and digits.
- `1 <= timestamp <= 107`
- All the timestamps `timestamp` of `set` are strictly increasing.
- At most `2 * 105` calls will be made to `set` and `get`.



**Solution:**

```java
class TimeMap {
    Map<String, List<Pair<String, Integer>>> map;
  // "foo", {["bar",  1]}
  // "foo", {["bar", 1], ["bar2", 4]}

    public TimeMap() {
        map = new HashMap<String, List<Pair<String, Integer>>>();
    }
    
    public void set(String key, String value, int timestamp) {
        map.putIfAbsent(key, new ArrayList<Pair<String, Integer>>());
      	
        // Store '(timestamp, value)' pair in 'key' bucket;
        map.get(key).add(new Pair(value, timestamp));
    }
  // TC: O(n)
    
    public String get(String key, int timestamp) {
        String res = "";
 
        if (map.containsKey(key)){
            List<Pair<String, Integer>> temp = map.get(key);

          	// Using binary search on the list of pairs.
            int left = 0;
            int right = temp.size() - 1;

            while(left <= right){
                int mid = left + (right - left)/2;

                if (temp.get(mid).getValue() <= timestamp){
                    res = temp.get(mid).getKey();
                    left = mid + 1;
                }else{
                    right = mid - 1;
                }
            }
        }
				// If the 'key' does not exist in map we will return empty string
        return res;
    }
  
  // TC: O(nlogk) // k = we use binary search on the key's bucket which 
  // have at most M elelements and to hash the string it takes O(L) time.
}

/**
 * Your TimeMap object will be instantiated and called as such:
 * TimeMap obj = new TimeMap();
 * obj.set(key,value,timestamp);
 * String param_2 = obj.get(key,timestamp);
 */

// TC: O(nlogk)
// SC: O(n)
```

https://www.youtube.com/watch?v=fu2cD_6E8Hw


| key   | Values<val, time>       |
| ----- | ----------------------- |
| "foo" | <"bar", 1>, <"bar2", 2> |
|       |                         |



```java
class TimeMap {
    public Map<String, List<Pair<String, Integer>>> map;
    public TimeMap() {
        this.map = new HashMap<>();
    }
    
    public void set(String key, String value, int timestamp) {
        map.putIfAbsent(key, new ArrayList<>());

        map.get(key).add(new Pair(value, timestamp));
    }
    
    public String get(String key, int timestamp) {
        String result = "";

        if (map.containsKey(key)){
            List<Pair<String, Integer>> temp = map.get(key); // ["bar", 1] ["bar2", 4]
            for (int i = temp.size() - 1; i >= 0; i--){
                if (temp.get(i).getValue() <= timestamp){
                    return temp.get(i).getKey();
                }
            }
        }

        return result;
    }
}
// TC: O(nlogn)
// SC: O(n)
/**
 * Your TimeMap object will be instantiated and called as such:
 * TimeMap obj = new TimeMap();
 * obj.set(key,value,timestamp);
 * String param_2 = obj.get(key,timestamp);
 */

// TC: O(n)
// SC: O(n)
```

