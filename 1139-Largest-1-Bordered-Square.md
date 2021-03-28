# 1139. Largest 1-Bordered Square

给出一个01二位矩阵，求出最大的以1为边的正方形面积。

这题还是动态规划的思路，针对每个坐标`(i, j)`都求出以它为左上角的最大1边正方形的边长，最后我们就可以得到整个二维数组里最大的1边正方形大小。每个坐标都去从头开始计算肯定不行，时间复杂度太高，所以我们先提前计算好给定一个坐标`(i, j)`的右方和下方包括自己在内有多少个连续的1。正如我们上一题说的那样，右方和下方的连续1可以同时计算。

得到预计算好的数据之后，针对每个坐标`(i, j)`只要`grid[i][j]`为1的话，就满足正方形的条件了。接着我们取该点右方和下方最短的连续1长度作为初始边长`edge`。但是呢，以`(i, j)`为左上角的最大1边正方形不一定就是这个边长，真正的边长还得再联系另外两个顶点。对于所有的`0 <= k <= edge`看看`(i + k - 1, j)`包括自己在内右方有多少个连续1，`(i, j + k - 1)`包括自己在内下方有多少个连续1，也就是`right[i + k - 1][j]`和`down[i][j + k - 1]`。这样我们就可以求出以`(i, j)`为左上角的最大1边正方形的边长，最后我们也就可以得到整个矩阵里最大的1边正方形边长。

返回边长的平方作为面积。

Time complexity: O(M * N)

Space complexity: O(M * N)


```java
class Solution {
  public int largest1BorderedSquare(int[][] grid) {
    if (grid.length == 0 || grid[0].length == 0) {
      return 0;
    }
    List<int[][]> rightDown = calculateRightDown(grid);
    int[][] right = rightDown.get(0), down = rightDown.get(1);
    int m = grid.length, n = grid[0].length;
    int maxLen = 0;
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        if (grid[i][j] == 1) {
          int edge = Math.min(right[i][j], down[i][j]);
          for (int k = edge; k >= 0; k--) {
            if (right[i + k - 1][j] >= k && down[i][j + k - 1] >= k) {
              edge = k;
              break;
            }
          }
          maxLen = Math.max(maxLen, edge);
        }
      }
    }
    return maxLen * maxLen;
  }

  private List<int[][]> calculateRightDown(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    int[][] right = new int[m][n];
    int[][] down = new int[m][n];
    for (int i = m - 1; i >= 0; i--) {
      for (int j = n - 1; j >= 0; j--) {
        if (grid[i][j] == 1) {
          if (i == m - 1 && j == n - 1) {
            right[i][j] = 1;
            down[i][j] = 1;
          } else if (i == m - 1) {
            right[i][j] = right[i][j + 1] + 1;
            down[i][j] = 1;
          } else if (j == n - 1) {
            right[i][j] = 1;
            down[i][j] = down[i + 1][j] + 1;
          } else {
            right[i][j] = right[i][j + 1] + 1;
            down[i][j] = down[i + 1][j] + 1;
          }
        }
      }
    }
    List<int[][]> rightDown = new ArrayList<>();
    rightDown.add(right);
    rightDown.add(down);
    return rightDown;
  }
}
```