---
tags:
    - Array
    - Sorting
    - Top Interviews
---

# [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return *an array of the non-overlapping intervals that cover all the intervals in the input*.

 

**Example 1:**

```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
```

**Example 2:**

```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

 

**Constraints:**

- `1 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 104`



**Solution:**

以示例 1 为例，我们有 [1,3],[2,6],[8,10],[15,18] 这四个区间。

为方便合并，把区间按照左端点从小到大排序（示例 1 已经按照左端点排序了）。排序的理由会在下面的合并过程中说明。

排序后，我们就知道了第一个合并区间的左端点，即 intervals[0][0]=1。

第一个合并区间的右端点是多少？目前只知道其 ≥intervals[0][1]=3，但具体是多少现在还不确定，得向右遍历。

具体算法如下：

把 intervals[0] 加入答案。注意，答案的最后一个区间表示当前正在合并的区间。
遍历到 intervals[1]=[2,6]，由于左端点 2 不超过当前合并区间的右端点 3，可以合并。由于右端点 6>3，那么更新当前合并区间的右端点为 6。注意，由于我们已经按照左端点排序，所以 intervals[1] 的左端点 2 必然大于等于合并区间的左端点，所以无需更新当前合并区间的左端点。
遍历到 intervals[2]=[8,10]，由于左端点 8 大于当前合并区间的右端点 6，无法合并（两个区间不相交）。再次利用区间按照左端点排序的性质，更后面的区间的左端点也大于 6，无法与当前合并区间相交，所以当前合并区间 [1,6] 就固定下来了，把新的合并区间 [8,10] 加入答案。
遍历到 intervals[3]=[15,18]，由于左端点 15 大于当前合并区间的右端点 10，无法合并（两个区间不相交），我们找到了一个新的合并区间 [15,18] 加入答案。
上述算法同时说明，按照左端点排序后，合并的区间一定是 intervals 中的连续子数组。

答疑
问：能不能按照右端点排序？

答：可以，但是需要倒着遍历 intervals 数组。如果正着遍历，比如 [1,2],[4,5],[1,6] 这三个区间，正确答案是合并成 [1,6]，但正着遍历到 [4,5] 这个区间时，无法知道 [4,5] 能否和 [1,2] 彻底断开。但按左端点排序的话，我们就知道这是不会断开的，会和之前的区间合并在一起。

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        // base case
        if (intervals.length <= 1){
            return intervals;
        }

        List<int[]> result = new ArrayList<>();

        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

        for (int[] p : intervals){
            int m = result.size();

            if (m > 0 && p[0] <= result.get(m - 1)[1]){
                result.get(m-1)[1] = Math.max(result.get(m - 1)[1], p[1]);
            }else{
                result.add(p);
            }
        }

        return result.toArray(new int[result.size()][]);
    }
}

// TC: O(nlogn)
// SC: O(1) 空间复杂度：O(1)。排序的栈开销和返回值不计入。
```



```java
class Solution {
    public int[][] merge(int[][] intervals) {
        // base case
        if (intervals.length <= 1){
            return intervals;
        }

        List<int[]> result = new ArrayList<>();

        Arrays.sort(intervals, (a,b) -> Integer.compare(a[0], b[0]));

        int[] newInterval = intervals[0];
        for (int i = 1; i < intervals.length; i++){
            int[] interval = intervals[i];
            if (newInterval[1] < interval[0]){
                result.add(newInterval);
                newInterval = interval;
            }else{
                // overlapping
                newInterval[0] = Math.min(newInterval[0], interval[0]);
                newInterval[1] = Math.max(newInterval[1], interval[1]);
            }
        }

        result.add(newInterval);



        return result.toArray(new int[result.size()][]);
    }
}
// TC: O(nlogn)
// SC: O(n) /O(1)
```



```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if (intervals.length <= 1) {
            return intervals;
        }

        // Sort intervals by the start time
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));

        List<int[]> result = new ArrayList<>();
        int[] newInterval = intervals[0];
        result.add(newInterval); // store address

        for (int[] interval : intervals) {
            if (interval[0] <= newInterval[1]) {  // Check if there is an overlap
                newInterval[1] = Math.max(newInterval[1], interval[1]); // Merge the interval
            } else { // No overlap
                newInterval = interval; // Move to the next interval
                result.add(newInterval);
            }
        }

        return result.toArray(new int[result.size()][]);
    }
}

// TC: O(nlogn)
// SC: O(n) / O(1) 空间复杂度：O(1)。排序的栈开销和返回值不计入。
```





> Integer.Compare
>
> vs Compareto?



> `        return result.toArray(new int[result.size()][]);`
>
> ?



https://www.bilibili.com/video/BV1Qx4y1E7zj/?t=2m56s&vd_source=73e7d2c4251a7c9000b22d21b70f5635

