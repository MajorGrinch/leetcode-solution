# Laicode 233. N Queens

N皇后问题，要求每一行、每一列和每一个斜对角线只能有一个皇后。

经典DFS题目了，这里我们可以用三个boolean数组来快速判断皇后是否可以放在一个位置。假设该位置坐标是`(row, col)`，那么该列就只能有这一个皇后，所以`used[col]`不能为true。该对角线也只能有一个皇后，根据观察，我们知道一个对角线上的坐标和是相同的，即`x1 + y1 == x2 + y2`。所以对应的`used[row + col]`也不能为true。另一个反对角线上也只能有一个皇后，根据观察我们知道同一个反对角线上坐标之差是相同的，即`x1 - y1 == x2 - y2`。所以对应的`used[row - col]`也不能为true。但是这里有一个需要注意的地方，`row-col`不能小于0，否则数组访问越界，我们用`n-1`作为offset来加上`row - col`的值。因为`row - col`的最小值也就是`-(n-1)`，所以这样可以阻止越界。

一行一行的尝试，对于每一列的位置，按照以上方法判断当前位置是否可以放置皇后。如果可以，就加入`rowQueens`作为答案备选，并且mark该列以及两个对角线以防止以后新的皇后与它冲突。并向下一层继续搜索，直到搜完所有的行，再把所有的N皇后位置放入最终的答案组合。

Time complexity: O(n * n!)

Space complexity: O(n), because we need extra space to mark and n level call stack.


```java
public class Solution {
  public List<List<Integer>> nqueens(int n) {
    List<List<Integer>> res = new ArrayList<>();
    int[] rowQueens = new int[n];
    boolean[] usedCol = new boolean[n];
    boolean[] usedDiag = new boolean[2 * n - 1];
    boolean[] usedRevDiag = new boolean[2 * n - 1];
    dfs(0, n, rowQueens, usedCol, usedDiag, usedRevDiag, res);
    return res;
  }

  private void dfs(
      int row,
      int n,
      int[] rowQueens,
      boolean[] usedCol,
      boolean[] usedDiag,
      boolean[] usedRevDiag,
      List<List<Integer>> res) {
    if (row == n) {
      List<Integer> list = new ArrayList<>();
      for (int pos : rowQueens) list.add(pos);
      res.add(list);
      return;
    }
    for (int col = 0; col < n; col++) {
      if (isValid(row, col, n, usedCol, usedDiag, usedRevDiag)) {
        mark(row, col, n, usedCol, usedDiag, usedRevDiag);
        rowQueens[row] = col;
        dfs(row + 1, n, rowQueens, usedCol, usedDiag, usedRevDiag, res);
        unmark(row, col, n, usedCol, usedDiag, usedRevDiag);
      }
    }
  }

  private boolean isValid(
      int row, int col, int n, boolean[] usedCol, boolean[] usedDiag, boolean[] usedRevDiag) {
    return !usedCol[col] && !usedDiag[row + col] && !usedRevDiag[row - col + n - 1];
  }

  private void mark(
      int row, int col, int n, boolean[] usedCol, boolean[] usedDiag, boolean[] usedRevDiag) {
    usedCol[col] = true;
    usedDiag[row + col] = true;
    usedRevDiag[row - col + n - 1] = true;
  }

  private void unmark(
      int row, int col, int n, boolean[] usedCol, boolean[] usedDiag, boolean[] usedRevDiag) {
    usedCol[col] = false;
    usedDiag[row + col] = false;
    usedRevDiag[row - col + n - 1] = false;
  }
}
```