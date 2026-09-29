---
tags:
    - Array
    - Backtracking
    - Matrix
    - Top Interviews
---

# [79. Word Search](https://leetcode.com/problems/word-search/)

Given an `m x n` grid of characters `board` and a string `word`, return `true` *if* `word` *exists in the grid*.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

 

**Example 1:**

![img](./79. Word Search/word2.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true
```

**Example 2:**

![img](./79. Word Search/word-1.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true
```

**Example 3:**

![img](./79. Word Search/word3.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false
```



**Solution:**

```java
class Solution {
    int n;
    int m;
    int[][] dirs = {{1,0}, {-1, 0}, {0, 1}, {0, -1}};
    public boolean exist(char[][] board, String word) {
        n = board.length;
        m = board[0].length;
        boolean[][] visited = new boolean[n][m];
        for (int i = 0; i < n; i++){
            for (int j = 0; j < m; j++){
                char cur = board[i][j];
                if (cur == word.charAt(0)){
                    boolean result = dfs(0, i, j, board, word, visited);
                    if (result == true){
                        return true;
                    }
                }
            }
        }
        return false;
    }

    private boolean dfs(int index, int x, int y, char[][] board, String word, boolean[][] visited){
        if (x < 0 || x >= n || y < 0 || y >= m || visited[x][y] == true || index > word.length() || board[x][y] != word.charAt(index)){
            return false;
        }

        visited[x][y] = true;

        if (index == word.length() - 1){
            return true;
        }
        
        for (int[] dir : dirs){
            int newX = x + dir[0];
            int newY = y + dir[1];
            boolean subResult = dfs(index + 1, newX, newY, board, word, visited);
            if (subResult  == true){
                return true;
            }
        }


        visited[x][y] = false;
        return false;


    }
}


// TC: O((n*m)*4^(n*m))
// SC: O(n*m)
```



```java
class Solution {
    public boolean exist(char[][] board, String word) {
        // base case
        if (board == null || board.length == 0 || board[0].length == 0){
            return false;
        }

        int row = board.length;
        int col = board[0].length;

        boolean[][] visited = new boolean[row][col];

        char[] wordArray = word.toCharArray();

        for (int i = 0; i < row; i++){ // row
            for (int j = 0; j < col; j++){ // col
                int index = 0;
                if (dfs(board, wordArray, index, i, j, visited) == true){
                    return true;
                }
            }
        } 
        return false;
    }

    private boolean dfs(char[][] board, char[] wordArray, int index, int x, int y, boolean[][] visited){
        if (index == wordArray.length){
            return true;
        }

        int row = board.length;
        int col = board[0].length;

        if (x >= row || x < 0 || y >= col || y < 0 || visited[x][y] == true || wordArray[index] != board[x][y]){
            return false;
        }

        visited[x][y] = true;

        if (dfs(board, wordArray, index+1, x+1, y, visited) ||
        dfs(board, wordArray, index + 1, x-1, y, visited)||
        dfs(board, wordArray, index +1, x, y-1, visited)||
        dfs(board, wordArray, index + 1, x, y+1, visited)){
            return true;
        }

        visited[x][y] = false;
        return false;

    }
}


// TC: O(row * col * (dfs)) = O(row * col * (branch ^ level)) = O(row * col * 4 ^ length)
// SC: O(Math.max(row * col, length)) 
```



```java
class Solution {
    public boolean exist(char[][] board, String word) {
        // base case 
        if (board == null || board.length == 0 || board[0] == null || board[0].length == 0){
            return false;
        }

        int row = board.length;
        int col = board[0].length; 

        boolean[][] visited = new boolean[row][col];
        char[] array = word.toCharArray();

        for (int i = 0; i < row; i++){ /// where to start
            for (int j = 0; j < col; j++){
                int index = 0;
                if (dfs(board, index, array, i, j, visited)){
                    return true;
                }
            }
        }
        return false;
        
    }

    private boolean dfs(char[][] board, int index, char[] array, int x, int y, boolean[][] visited){
        int row = board.length;
        int col = board[0].length;

        if (index == array.length){
            return true;
        }

        // base case check
        if (x >= row || x < 0 || y >= col || y < 0 || visited[x][y] || array[index] != board[x][y]){
            return false;
        }

        visited[x][y] = true;

        // go int next level four directions
        if (dfs(board, index + 1, array, x + 1, y, visited) ||
        dfs(board, index+1, array, x -1 , y , visited) || 
        dfs(board, index+1, array, x, y - 1, visited) || 
        dfs(board, index + 1, array, x, y + 1, visited)){
            return true;
        }
        visited[x][y] = false;
        return false;
    }
}

// TC: O(row * col * 4 ^ length)
// SC: O(row * col)
```





