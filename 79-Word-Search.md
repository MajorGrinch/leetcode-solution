# 79. Word Search

DFS经典题目，针对每个位置进行深搜，如果有的话肯定能搜到。

Time complexity: O(m * n * 3^L) where L is the length of string.

Space complexity: O(L).

```java
class Solution {

  private int[] dr = {-1, 1, 0, 0};
  private int[] dc = {0, 0, -1, 1};

  public boolean exist(char[][] board, String word) {
    if (board.length == 0 || board[0].length == 0) {
      return false;
    }

    int m = board.length, n = board[0].length;
    boolean[][] vis = new boolean[m][n];
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        if (dfs(board, word, 0, i, j, vis)) return true;
      }
    }
    return false;
  }

  private boolean dfs(char[][] board, String word, int index, int r, int c, boolean[][] vis) {
    if (index == word.length()) {
      return true;
    }
    if (r < 0 || r >= board.length || c < 0 || c >= board[0].length) return false;

    if (vis[r][c] || word.charAt(index) != board[r][c]) return false;

    vis[r][c] = true;
    boolean found = false;
    for (int k = 0; k < dr.length; k++) {
      if (dfs(board, word, index + 1, r + dr[k], c + dc[k], vis)) return true;
    }
    vis[r][c] = false;
    return false;
  }
}
```