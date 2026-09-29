---
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Topological Sort
    - Top Interviews
---

# [210. Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return *the ordering of courses you should take to finish all courses*. If there are many valid answers, return **any** of them. If it is impossible to finish all courses, return **an empty array**.

 

**Example 1:**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].
```

**Example 2:**

```
Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].
```

**Example 3:**

```
Input: numCourses = 1, prerequisites = []
Output: [0]
```



**Solution:**

这个问题是拓扑排序问题可以用dfs 和 

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        int[] result = new int[numCourses];

        Map<Integer, List<Integer>> graph = new HashMap<>();

        for (int i = 0; i < numCourses; i++){
            graph.put(i, new ArrayList<>());
        }

        for (int[] prerequisite : prerequisites){
            int course = prerequisite[0];
            int pre = prerequisite[1];
            graph.get(course).add(pre);
        }

        Set<Integer> visited = new HashSet<>();

        Set<Integer> curPath = new HashSet<>();

        List<Integer> subResult = new ArrayList<>();

        for (int i = 0; i < numCourses; i++){
            if (dfs(i, graph, visited, curPath, subResult) == true){
                return new int[]{}; // can't return result, result might 
            }
        }

        for (int i = 0; i < subResult.size(); i++){
            result[i] = subResult.get(i);
        }

        return result;


    }

    private boolean dfs(int course, Map<Integer, List<Integer>> graph, Set<Integer> visited, Set<Integer> curPath, List<Integer> subResult){
        if (curPath.contains(course)){
            return true;
        }

        if (visited.contains(course)){
            return false;
        }
      
        visited.add(course);
        curPath.add(course);
        for (int next : graph.get(course)){
            if (dfs(next, graph, visited, curPath, subResult) == true){
                return true;
            }
        }

        curPath.remove(course);
        subResult.add(course);

        return false;
    }
}

// TC: O(V+E)
// SC: O(V + E)
```



```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
       int[] ans = new int[numCourses]; // [0, 1, 2, 3]

       Map<Integer, List<Integer>> preMap = new HashMap<>();
       // 0, []
       // 1, [0]
       // 2, [0]
       // 3, [1,2]

       for (int i = 0; i < numCourses; i++){
        preMap.put(i, new ArrayList<Integer>());
       }

       for (int[] row : prerequisites){// [1,0]//[2,0] //[3,1]// [3,2]
        int course = row[0]; // 1 // 2  // 3
        int prereq = row[1];// 0 //  0 // 1

        // List<Integer> allPrereq = preMap.get(course); // 1,[]
        // allPrereq.add(prereq);// 1, [0]
        // preMap.put(course, allPrereq); //  1,[0]

    

        preMap.get(course).add(prereq); // 

       }
       // 0, []
       // 1, [0]
       // 2, [0]
       // 3, [1,2]

       Set<Integer> visited = new HashSet<>();
       // Set to keep track of visited nodes
       Set<Integer> cycle = new HashSet<>();
       // Set to detect cycles in the graph.

       List<Integer> ansList = new ArrayList<>();
       // List to store the courses in the order of completion.

       for (int i = 0; i < numCourses; i++){
        if (!dfs(i, preMap, ansList, visited, cycle)){
            return new int[]{};
        }
       }

       for (int i = 0; i < ansList.size(); i++){
        ans[i] = ansList.get(i);
       }

       return ans;
    }

    public boolean dfs(int course, Map<Integer, List<Integer>> preMap, List<Integer> ansList, Set<Integer> visited, Set<Integer> cycle){
        if (cycle.contains(course)){
            return false;
        }

        // If the course is in the cycle set, we found a cycle, return false.

        if (visited.contains(course)){
            return true;
        }
        // If the course is already visited, no need to process it again.

        cycle.add(course);

        for(int prereq : preMap.get(course)){
            if (!dfs(prereq, preMap, ansList, visited, cycle)){
                return false;
            }
        }
        // Recursively perform DFS on all prerequisites of the course.

        cycle.remove(course);
        // Remove the course from the cycle set as we're done processing it.
        visited.add(course);
        // Mark the course as visited.
        ansList.add(course);
        // Return true as the course can be completed.
        return true;
    }
}
// TC: O(V+E)
// SC: O(V+E)
```





###Kahn's Algorithm

基于(BFS)

核心思想: 做完一件事打勾

步骤:

1. 统计所有点的入度
2. 执行入度为0的点代表的任务
3. 每执行完一个任务, 所有后继任务的入度减1
4. 重复2和3直到所有可执行任务都被执行完
5. 如果还有剩余任务, 返回无解, 否则返回任务执行顺序



```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        // 创建图和入度数组
        Map<Integer, List<Integer>> graph = new HashMap<>();
        int[] inDegree = new int[numCourses];
        int[] result = new int[numCourses]; // 用于存储拓扑排序结果

        // 初始化图结构
        for (int i = 0; i < numCourses; i++) {
            graph.put(i, new ArrayList<>());
        }

        // 填充图和计算入度
        for (int[] prerequisite : prerequisites) {
            int course = prerequisite[0];
            int pre = prerequisite[1];
            graph.get(pre).add(course); // pre 依赖 course
            inDegree[course]++; // 记录 course 的入度
        }

        // 使用队列存储所有入度为 0 的节点
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) {
                queue.offer(i); // 入度为 0 的节点可以作为起点
            }
        }

        int index = 0; // 记录排序位置

        // 进行拓扑排序
        while (!queue.isEmpty()) {
            int course = queue.poll();
            result[index++] = course; // 将课程添加到结果数组中

            // 遍历该课程的邻居节点
            for (int neighbor : graph.get(course)) {
                inDegree[neighbor]--;
                if (inDegree[neighbor] == 0) {
                    queue.offer(neighbor); // 入度为 0 的邻居节点可以入队
                }
            }
        }

        // 如果结果中的课程数量不等于 numCourses，说明存在环，无法完成所有课程
        if (index != numCourses) {
            return new int[0]; // 返回空数组表示不能完成所有课程
        }

        return result; // 返回拓扑排序结果
    }
}
```



https://www.youtube.com/watch?v=Akt3glAwyfY&t=145s

https://www.bilibili.com/video/BV1PZ4y1X7w4/?spm_id_from=333.337.search-card.all.click&vd_source=73e7d2c4251a7c9000b22d21b70f5635
