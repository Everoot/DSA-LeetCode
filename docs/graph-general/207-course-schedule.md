---
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Topological Sort
    - Top Interviews
---

# [207. Course Schedule](https://leetcode.com/problems/course-schedule/)

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

 

**Example 1:**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
```

**Example 2:**

```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
```



**Solution:**

DFS:

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        Map<Integer, List<Integer>> graph = buildGraph(numCourses, prerequisites);
        Set<Integer> visited = new HashSet<>();
        for (int i = 0; i < numCourses; i++){
            Set<Integer> curPath = new HashSet<>();
            if (dfs(i, graph, curPath, visited) == true){
                // find cycle
                return false;
            }
        }

        return true;
    }

    private boolean dfs(int numCourse, Map<Integer, List<Integer>> graph, Set<Integer> curPath, Set<Integer> visited){
        if (curPath.contains(numCourse)){
            return true;
        }

        if (visited.contains(numCourse)){
            return false;
        }

        curPath.add(numCourse);
        visited.add(numCourse);

        for (Integer nei : graph.get(numCourse)){
            if (dfs(nei, graph, curPath, visited)){
                return true;
            }
        }

        curPath.remove(numCourse);

        return false;
    }

    private Map<Integer, List<Integer>> buildGraph(int numCourses, int[][] prerequisites){
        Map<Integer, List<Integer>> graph = new HashMap<>();
        for (int i = 0; i < numCourses; i++){
            graph.put(i, new ArrayList<>());
        }

        for (int[] prerequisite : prerequisites){
            int cur = prerequisite[0];
            int pre = prerequisite[1];

            graph.get(cur).add(pre);
        }

        return graph;
    }
}

/*
 1, 0 
 
 0 -> 1

[1,0],[0,1]

1-> 0
  <-


*/

// TC: O(V+E)
// SC: O(V+E)
// TC: O(V + E)
// SC: O(V + E)
```



如果在递归过程中，发现下一个节点在递归栈中（正在访问中），则找到了环。

![lc207.png](./207. Course Schedule/1731932427-qCTQYV-lc207.png)

例如 1→2→3→4→2，走到 4 的时候，发现下一个节点 2 在递归栈中（正在访问中），那么就找到了环。

注：我们说节点 x「正在访问中」，是说我们正在递归处理节点 x 以及它的后续节点，dfs(x) 尚未结束。

具体思路
对于每个节点 x，都定义三种颜色值（状态值）：

0：节点 x 尚未被访问到。
1：节点 x 正在访问中，dfs(x) 尚未结束。
2：节点 x 已经完全访问完毕，dfs(x) 已返回。

⚠误区：不能只用两种状态表示节点「没有访问过」和「访问过」。例如上图，我们先 dfs(0)，再 dfs(1)，此时 1 的邻居 0 已经访问过，但这并不能表示此时就找到了环。



```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // Create Array of Lists -> adj matrix of graph
        // 这个列表的每个元素都是一个列表，用来表示图的邻接表
        List<List<Integer>> adj = new ArrayList<List<Integer>>(numCourses);
        //图中的每个顶点对应一门课程，每个顶点的邻接表存储了所有需要先修的课程

        for (int i = 0; i < numCourses; i++){
            adj.add(new ArrayList<>());
        }// [[], [], [], [],[] ]


        for (int[] prerequisite : prerequisites){
            adj.get(prerequisite[1]).add(prerequisite[0]);
        }
        // [[0,1],[0,2],[1,3],[1,4],[3,4]]
        // [0,1]    prerequisite[1] = 1, preequisite[0] =  [[], [0], [], [],[] ]
        // index i need is adj.get(i) prerequisite
        // 遍历每个先修关系, 构建图的邻接表: 对于先修关系[a,b]
        // 将a添加到b的邻接表中, 表示要学习a, 你必须先完成b
     	 // Array index是课程adj.get[]的先修课 

        boolean[] visit = new boolean[numCourses];
        // 用于标记某个节点(课程) 在dfs过程中是否被访问
        boolean[] inStack = new boolean[numCourses];
        // 用于标记节点在当前DFS路径中是否出现, 以帮助检测环.

        for (int i = 0; i < numCourses; i++){
            if (dfs(i, adj, visit, inStack)){
                return false;
            }
        }
        // 遍历每个节点(课程), 使用dfs方法进行深度优先搜索. 如果发现环, 则表示课程安排不可能完成, 返回false

        return true;
    }

    public boolean dfs(int node, List<List<Integer>> adj, boolean[] visit, boolean[] inStack){
        if (inStack[node]){
            return true;
        }// 如果当前节点已经在栈中(即当前dfs路径上), 说明发现了一个环

        if (visit[node]){
            return false;
        }// 如果当前节点已经被访问过, 且没有在当前dfs路径上, 则没有发现环

        visit[node] = true;
        inStack[node] = true;
        // 标记当前节点为已经访问, 并将其加入当前dfs路径

        for (int neighbor : adj.get(node)){
            if (dfs(neighbor, adj, visit, inStack)){
                return true;
            }
        }
        // 遍历当前节点的所有邻接节点(即需要先修的课程), 递归地进行DFS. 如果在任何邻接节点的搜索中发现了环, 则当前
        // 的DFS耶应该返回true;

        inStack[node] = false;
        // 当前节点的所有邻接节点都被访问后, 将其从当前dfs路径中移除.
        
        return false;
    }
}
// TC: O(V+E) // prerequisites数组长度为E, E事变得数量, 徐阿姨哦为每个先修条件添加一条边
              //DFS: 需要访问图中的所有顶点和边. 每个顶点被访问一次, 每条边也在DFS过程中被考察一次. 因此, 这个步骤
							// 的时间复杂度是O(V+E), 其中V是顶点(课程)的数量, E是边(先修条件)的数量
// SC: O(V+E)
```





```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        int[] indegree = new int[numCourses];
        List<List<Integer>> adj = new ArrayList<List<Integer>>(numCourses);

        for (int i = 0; i < numCourses; i++){
            adj.add(new ArrayList<>());
        }

        for (int[] prerequisite : prerequisites){
            int course = prerequisite[0];
            int prerequisiteCourse = prerequisite[1];

            adj.get(prerequisiteCourse).add(course);
            indegree[prerequisite[0]]++;
        }

        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses;i ++){
            if (indegree[i] == 0){
                queue.offer(i);
            }
        }

        int nodesVisited = 0;
        while(!queue.isEmpty()){
            int node = queue.poll();
            nodesVisited++;

            for (int neighbor : adj.get(node)){
                indegree[neighbor]--;
                if (indegree[neighbor] == 0){
                    queue.offer(neighbor);
                }
            }
        }

        return nodesVisited == numCourses;
        
    }
}
// TC: O(m+n)
// SC: O(m+n)
```

https://leetcode.com/explore/learn/card/graph/623/kahns-algorithm-for-topological-sorting/3886/

<img src="./207. Course Schedule/Screenshot 2024-01-14 at 17.51.52.png" alt="Screenshot 2024-01-14 at 17.51.52" style="zoom:50%;" />





### Kahn's

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        return kahnTopologicalSort(numCourses, prerequisites);
    }

    private boolean kahnTopologicalSort(int numCourses, int[][] prerequisites) {
        // 创建图和入度数组 因为入度
        Map<Integer, List<Integer>> graph = new HashMap<>();
        int[] inDegree = new int[numCourses];

        // 初始化图结构
        for (int i = 0; i < numCourses; i++) {
            graph.put(i, new ArrayList<>());
        }

        // 填充图和计算每个节点的入度
        for (int[] prerequisite : prerequisites) {
            int course = prerequisite[0];
            int pre = prerequisite[1];
            graph.get(pre).add(course); // 课程依赖关系 因为入度
          // pre -> course
            inDegree[course]++; // 记录课程的入度
        }

        // 使用队列存储入度为 0 的节点
        Queue<Integer> queue = new LinkedList<>();

        // 将所有入度为 0 的课程加入队列
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) {
                queue.offer(i);
            }
        }

        int count = 0; // 用来记录处理过的节点数

        // 处理队列中的节点
        while (!queue.isEmpty()) {
            int course = queue.poll();
            count++;

            // 遍历该课程的邻居节点，将它们的入度减 1
            for (int neighbor : graph.get(course)) {
                inDegree[neighbor]--;
                // 如果入度为 0，则加入队列
                if (inDegree[neighbor] == 0) {
                    queue.offer(neighbor);
                }
            }
        }

        // 如果所有节点都被处理了，则说明无环，否则有环
        return count == numCourses;
    }
}

```

