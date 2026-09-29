---
tags:
    - Array
    - Heap (Priority Queue)
    - Top Interviews
---

# [373. Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

You are given two integer arrays `nums1` and `nums2` sorted in **non-decreasing order** and an integer `k`.

Define a pair `(u, v)` which consists of one element from the first array and one element from the second array.

Return *the* `k` *pairs* `(u1, v1), (u2, v2), ..., (uk, vk)` *with the smallest sums*. 

**Example 1:**

```
Input: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
Output: [[1,2],[1,4],[1,6]]
Explanation: The first 3 pairs are returned from the sequence: [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
```

**Example 2:**

```
Input: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
Output: [[1,1],[1,1]]
Explanation: The first 2 pairs are returned from the sequence: [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]
```

**Example 3:**

```
Input: nums1 = [1,2], nums2 = [3], k = 3
Output: [[1,3],[2,3]]
Explanation: All possible pairs are returned from the sequence: [1,3],[2,3]
```



**Solution:**

**一、初步思路**
为描述方便，下文把 $nums_1$记作 a，$nums_2$记作 b。

哪个数对的和最小？

由于数组是有序的，$(a[0],b[0])$ 是和最小的数对，计入答案。

哪个数对的和第二小？

次小只能是 $(a[0],b[1])$ 或 $(a[1],b[0])$，其它没有计入答案的数对和不会比这两个更小。

$(a[0],b[1])$ 与 $(a[1],b[0])$ 这两个数对和的大小还好比较，但如果要求第 k 小，就要涉及到更多的数对，那样就更加复杂了。如何按从小到大的顺序快速地求出这些数对呢？

**二、借助最小堆**

为了更高效地比大小，我们可以借助最小堆来优化。

堆中保存下标对 $(i,j)$，即可能成为下一个数对的 $a$ 的下标 $i$ 和 $b$ 的下标 $j$。堆顶是最小的$a[i]+b[j]$。

初始把 $(0,0)$ 入堆。

每次 $(i,j)$ 出堆时，把候选项 $(i+1,j)$ 和 $(i,j+1)$ 入堆。（和「初步思路」中的讨论一样，其它的不会比这两个更小。）

但这会导致一个问题：例如当 $(1,0)$ 出堆时，会把 $(1,1)$ 入堆；当 $(0,1)$ 出堆时，也会把 $(1,1)$ 入堆，这样堆中会有重复元素。为了避免有重复元素，还需要额外用一个哈希表记录在堆中的下标对。只有当下标对不在堆中时，才能入堆。

能否不用哈希表呢？

**三、优化**
换个角度，如果要把 $(i,j)$ 入堆，那么之前出堆的下标对是什么？

根据上面的讨论，出堆的下标对只能是 $(i−1,j)$ 和 $(i,j−1)$。

只要保证 $(i−1,j)$ 和 $(i,j−1)$ 的其中一个会将 $(i,j)$ 入堆，而另一个什么也不做，就不会出现重复了！

不妨规定 $(i,j−1)$ 出堆时，将 $(i,j)$ 入堆；而 $(i−1,j)$ 出堆时只计入答案，其它什么也不做。

换句话说，在 $(i,j)$ 出堆时，只需将 $(i,j+1)$ 入堆，无需将 $(i+1,j)$ 入堆。

但若按照该规则，初始仅把 $(0,0)$ 入堆的话，只会得到 $(0,1),(0,2),⋯ $这些下标对。

所以初始不仅要把 $(0,0)$ 入堆，$(1,0),(2,0),⋯$ 这些都要入堆。

代码实现时，为了方便比较大小，实际入堆的是三元组 $(a[i]+b[j],i,j)$。

### 另一种理解角度

示例 1 的 $nums_1=[1,7,11]$，$nums_2=[2,4,6]$。我们把每个数对的和算出来，可以得到一个矩阵$M$，其中 $M_{i,j} =nums_1[i]+nums_2[j]$。
$$
M = \left [ \begin{matrix}
3 & 5 & 7 \\
9 & 11 & 13 \\
13 & 15 & 17
\end{matrix} \right ]
$$
由于 $nums_2$是递增的，所以矩阵每一行都是递增的。问题相当于：

* 合并 $n$ 个升序列表，找前 k 小元素。（其中$n$ 是 $nums_1$ 的长度）

根据 23. 合并 K 个升序链表 的 堆的做法：

1. 把矩阵每一行的第一个数 $M_{i,0}$及其位置 $(i,0)$ 加到最小堆中。
2. 循环 k 次。
3. 每次循环，弹出堆顶，把堆顶 $M_{i,j}$的对应数对加入答案，把堆顶右边元素$M_{i,j+1}$及其位置 $(i,j+1)$ 入堆。



```java
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
       List<List<Integer>> result = new ArrayList<>();

       PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> (a[2] - b[2]));

       //    2 4 6
       // 1  3
       // 7  9
       // 11 13
       for (int i = 0; i < Math.min(nums1.length, k); i++){
        pq.offer(new int[]{i, 0, nums1[i] + nums2[0]});
       }

       for (int i = 0; i < k; i++){
        int[] cur = pq.poll();
        int x = cur[0];
        int y = cur[1];
        result.add(List.of(nums1[x], nums2[y]));
        if (y + 1 < nums2.length){
            pq.offer(new int[]{x, y + 1, nums1[x] + nums2[y + 1]});
        }
       }

       return result;
    }
}
```



```java
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
       List<List<Integer>> result = new ArrayList<>(k); // 预分配空间
       PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);

       for (int i = 0; i < Math.min(nums1.length, k); i++){ // 至多 k 个
        pq.add(new int[]{nums1[i] + nums2[0], i, 0});// {1+2, 0, 0}, {7 + 2, 1, 0}, {11 + 2, 2, 0}
        // {3, 0, 0}, {9, 1, 0}, {13, 2, 0}
       } 
       for (int z = 0; z < k; z++){
        int[] top = pq.poll();
        int i = top[1];
        int j = top[2];
        result.add(List.of(nums1[i], nums2[j]));
        if (j + 1 < nums2.length){
            pq.add(new int[]{nums1[i] + nums2[j + 1], i, j + 1});
        }
       }

       return result;
    }
}

// TC: O(klogmin(n,k))，其中 n 为 nums_1的长度。为了得到 k 个数对，需要循环 k 次，每次出堆入堆的时间复杂度为 logmin(n,k)。所以总的时间复杂度为 O(klogmin(n,k))。
// SC: O(min(n,k))。堆中至多有 O(min(n,k)) 个三元组。
```



```java
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        int n = nums1.length;
        int m = nums2.length;
        List<List<Integer>> result = new ArrayList<>();
        if (n == 0 || m == 0 || k == 0) return result; // edge case
        
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> 
            (nums1[a[0]] + nums2[a[1]]) - (nums1[b[0]] + nums2[b[1]])); // min heap => sorting by the pair sum
        
        for (int i = 0; i < Math.min(n, k); i++) {
            pq.offer(new int[]{i, 0}); // insert all pair of nums1[i] with nums2[0]
        }

        // Think of it as grid
        //           2    4    6  
        //  ------ | -- | -- | -- | --
        //  1      | 3  | 5  | 7  |
        //  7      | 9  | 11 | 13 |
        //  11     | 13 | 15 | 17 |

        while (k-- > 0 && !pq.isEmpty()) { // until k!=0 or pq != empty
            int[] curr = pq.poll(); // retrieve min
            int i = curr[0]; // index of nums1
            int j = curr[1]; // index of nums2

            result.add(new ArrayList<>(Arrays.asList(nums1[i], nums2[j]))); // add this pair to result
            
            if (j + 1 < m) { // next pair
                pq.offer(new int[]{i, j + 1});
            }
        }
        return result;
    }
}
```



```java
class Solution {
    static class Cell{
        int x;
        int y;
        int value;

        Cell(int x, int y, int value){
            this.x = x;
            this.y = y;
            this.value = value;
        }
    }

    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        int m = nums1.length;
        int n = nums2.length;

        List<List<Integer>> result = new ArrayList<>();

        //boolean[][] visited = new boolean[m][n];
        Set<Pair<Integer, Integer>> visited = new HashSet<Pair<Integer, Integer>>();

        PriorityQueue<Cell> minHeap = new PriorityQueue<Cell>(k, new Comparator<Cell>(){
            public int compare(Cell c1, Cell c2){
                if (c1.value == c2.value){
                    return 0;
                }else if (c1.value < c2.value){
                    return -1;
                }else{
                    return 1;
                }
            }
        });
        

        minHeap.offer(new Cell(0, 0, nums1[0]+ nums2[0]));
        // visited[0][0] = true;
        visited.add(new Pair<Integer,Integer>(0,0));

        for (int i = 0; i < k; i++){
            Cell cur = minHeap.poll();
            result.add(List.of(nums1[cur.x], nums2[cur.y]));

            if (cur.x + 1 < m && !visited.contains(new Pair<Integer, Integer>(cur.x+ 1, cur.y))){
                minHeap.offer(new Cell(cur.x + 1, cur.y, nums1[cur.x+1]+ nums2[cur.y]));
                // visited[cur.x+1][cur.y] = true;
                visited.add(new Pair<Integer, Integer>(cur.x+1, cur.y));
            }

            if (cur.y + 1 < n && !visited.contains(new Pair<Integer, Integer>(cur.x, cur.y + 1))){
                minHeap.offer(new Cell(cur.x, cur.y + 1, nums1[cur.x]+ nums2[cur.y+1]));
                visited.add(new Pair<Integer, Integer>(cur.x, cur.y + 1));
            }

        }

        return result;
    }
}
// TC: O(klogk)
// SC: O(k)
```



```java
class Solution {
    static class Cell{
        int x;
        int y;
        int value;

        Cell(int x, int y, int value){
            this.x = x;
            this.y = y;
            this.value = value;
        }
    }

    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        int m = nums1.length;
        int n = nums2.length;

        List<List<Integer>> result = new ArrayList<>();
        boolean[][] visited = new boolean[m][n];

        PriorityQueue<Cell> minHeap = new PriorityQueue<Cell>(k, new Comparator<Cell>(){
            public int compare(Cell c1, Cell c2){
                if (c1.value == c2.value){
                    return 0;
                }else if (c1.value < c2.value){
                    return -1;
                }else{
                    return 1;
                }
            }
        });

        minHeap.offer(new Cell(0, 0, nums1[0]+ nums2[0]));
        visited[0][0] = true;

        for (int i = 0; i < k; i++){
            Cell cur = minHeap.poll();
            result.add(List.of(nums1[cur.x], nums2[cur.y]));

            if (cur.x + 1 < m && !visited[cur.x+ 1][cur.y]){
                minHeap.offer(new Cell(cur.x + 1, cur.y, nums1[cur.x+1]+ nums2[cur.y]));
                visited[cur.x+1][cur.y] = true;
            }

            if (cur.y + 1 < n && !visited[cur.x][cur.y + 1]){
                minHeap.offer(new Cell(cur.x, cur.y + 1, nums1[cur.x]+ nums2[cur.y+1]));
                visited[cur.x][cur.y+1] = true;
            }

        }

        return result;
    }
}

// TC: O(klogk)
// SC: O(k)
```





