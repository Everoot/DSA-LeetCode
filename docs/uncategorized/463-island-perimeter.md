---
tags:
    - Mid Level
    - Array
    - Depth-First Search
    - Breadth-First Search
    - Matrix
    - Top Interviews
---

# [463. Island Perimeter](https://leetcode.com/problems/island-perimeter/)

You are given `row x col` `grid` representing a map where `grid[i][j] = 1` represents land and `grid[i][j] = 0` represents water.

Grid cells are connected **horizontally/vertically** (not diagonally). The `grid` is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).

The island doesn't have "lakes", meaning the water inside isn't connected to the water around the island. One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

 

**Example 1:**

![img](./463. Island Perimeter/island.png)

```
Input: grid = [[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]
Output: 16
Explanation: The perimeter is the 16 yellow stripes in the image above.
```

**Example 2:**

```
Input: grid = [[1]]
Output: 4
```

**Example 3:**

```
Input: grid = [[1,0]]
Output: 4
```

 

**Constraints:**

- `row == grid.length`
- `col == grid[i].length`
- `1 <= row, col <= 100`
- `grid[i][j]` is `0` or `1`.
- There is exactly one island in `grid`.



**Solution:**

```java
class Solution {
    public int islandPerimeter(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int ans = 0;
        for (int i = 0; i < m; i++){
            for (int j = 0; j < n; j++){
                if (grid[i][j] == 0){
                    continue;
                }
                
                if (i == 0 || grid[i - 1][j] == 0){
                    ans++;
                }

                if (i == m - 1 || grid[i + 1][j] == 0){
                    ans++;
                }

                if (j == 0 || grid[i][j - 1] == 0){
                    ans++;
                }
                
                if (j == n - 1 || grid[i][j + 1] == 0){
                    ans++;
                }
            }
        }

        return ans;
    }
}
// TC: O(nm)
// SC: O(nm)
```



```java
class Solution {
    public int islandPerimeter(int[][] grid) {
        int perimeter = 0;
        for (int i = 0; i < grid.length; i++){
            for (int j = 0; j < grid[0].length; j++){
                if (grid[i][j] == 1){
                    perimeter += dfs(i, j, grid);
                }
            }
        }

        return perimeter;
    }

    private int dfs(int x, int y, int[][] grid){
        if (x < 0 || x == grid.length || y < 0 || y == grid[0].length || grid[x][y] == 0){
            return 1;
        }

        if (grid[x][y] == -1){
            return 0;
        }

        grid[x][y] = -1;

        int perimeter = 0;
        perimeter += dfs(x + 1, y, grid);
        perimeter += dfs(x - 1, y, grid);
        perimeter += dfs(x, y + 1, grid);
        perimeter += dfs(x, y - 1, grid);

        return perimeter;
    }
}
```

