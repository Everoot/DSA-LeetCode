---
tags:
    - Array
    - Top Interviews
---

# [57. Insert Interval](https://leetcode.com/problems/insert-interval/)

You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the `ith` interval and `intervals` is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` *after the insertion*.

**Note** that you don't need to modify `intervals` in-place. You can make a new array and return it.

 

**Example 1:**

```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

**Example 2:**

```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

 

**Constraints:**

- `0 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 105`
- `intervals` is sorted by `starti` in **ascending** order.
- `newInterval.length == 2`
- `0 <= start <= end <= 105`



**Solution**:

```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        if (intervals.length == 0){
            return new int[][]{newInterval};
        }

        List<int[]> result = new ArrayList<>();

        //Arrays.sort(intervals, (a, b) -> (a[0] - b[0]));

        boolean isInsert = false;


        for (int[] interval : intervals){
            if (interval[1] < newInterval[0]){
                result.add(interval);
            }else if (interval[0] > newInterval[1]){
                if (isInsert == false){
                    result.add(newInterval);
                    isInsert = true;
                } 
                result.add(interval);
            }else{
                newInterval[0] = Math.min(newInterval[0], interval[0]);
                newInterval[1] = Math.max(newInterval[1], interval[1]);
            }
        }

        if (isInsert == false){
            result.add(newInterval);
        }

        return result.toArray(new int[result.size()][]);
    }
}

// TC: O(n)
// SC: O(1)
```



```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        List<int[]> result = new ArrayList<>();

        int[][] newIntervals = new int[intervals.length + 1][2];
        for (int i = 0; i < intervals.length; i++){
            newIntervals[i] = intervals[i]; 
        }
        newIntervals[intervals.length] = newInterval;
        Arrays.sort(newIntervals, (a, b) -> Integer.compare(a[0], b[0]));
        newInterval = newIntervals[0];
        for (int i = 0; i < newIntervals.length; i++){
            int[] cur = newIntervals[i];
            if (newInterval[1] < cur[0]){
                result.add(newInterval);
                newInterval = cur;
            }else{
                newInterval[1] = Math.max(newInterval[1] , cur[1]);
            }
        }

        result.add(newInterval);

        int[][] r = new int[result.size()][2];
        int i = 0;
        for (int[] res : result){
            r[i] = res;
            i++;
        }

        return r;
    }
}

// TC: O(nlogn)
// SC: O(n)
```



```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        List<int[]> result = new ArrayList<>();
        boolean inserted = false;
              // [1,3]   new [4, 5]  

        // new [0, 1] [4,5]

        for (int[] interval : intervals) {
            if (interval[1] < newInterval[0]) {
                // Current interval ends before the new interval starts
                result.add(interval);
            } else if (interval[0] > newInterval[1]) {
                // Current interval starts after the new interval ends
                if (!inserted) {
                    result.add(newInterval);
                    inserted = true;
                }
                result.add(interval);
            } else {
                // There is an overlap, merge intervals
              //             // [1,3]  new[2, 5]
                newInterval[0] = Math.min(newInterval[0], interval[0]);
                newInterval[1] = Math.max(newInterval[1], interval[1]);
                         // [1, 5] 
            }
        }

        // If newInterval has not been added, add it now
      //         // []     new [5, 7]
        if (!inserted) {
            result.add(newInterval);
        }

        // Convert ArrayList to array for returning
        return result.toArray(new int[result.size()][]);
    }
}

// TC: O(n)
// SC: O(1)
```

