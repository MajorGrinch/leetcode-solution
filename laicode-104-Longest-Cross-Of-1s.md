# Laicode 104. Longest Cross Of 1s

给定一个01二维矩阵，找出其中最大的全1十字架的尺寸。全1十字架是四边是等长的，如果不等长则取最短的那个边的长度作为该十字架的尺寸。

这题也依然可以用动态规划来解决，对于每个坐标`(i, j)`我们可以提前计算出`(i, j)`上下左右包括它自己在内各自有多少个1。有了这些数据之后，我们就可以计算以每个坐标为中心的十字架尺寸，从而可以得到最长的十字架尺寸。

左方和上方的`1`长度可以一起计算，我们可以从左上遍历到右下，这样子左方和上方就可以一起计算。同理，我们再从右下遍历到左上，那么下方和右方也可以一起计算。当然了，左方和上方可以合并到一起，右方和下方也可以合并到一起，取短边的长度就好。最后我们把左上和右下接着取短边合并到一起，就可以得到四边里面最短的，那么也就得到了以每个坐标为中心的十字架尺寸。

最后，我们就可以得到最大的十字架尺寸。

Time complexity: O(M * N)

Space complexity: O(M * N)

```java
public class Solution {
  public int largest(int[][] matrix) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
      return 0;
    }
    return merge(calculateLeftUp(matrix), calculateRightDown(matrix));
  }

  private int[][] calculateLeftUp(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    int[][] left = new int[m][n];
    int[][] up = new int[m][n];
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        if (matrix[i][j] == 1) {
          if (i == 0 && j == 0) {
            left[i][j] = 1;
            up[i][j] = 1;
          } else if (i == 0) {
            left[i][j] = left[i][j - 1] + 1;
            up[i][j] = 1;
          } else if (j == 0) {
            left[i][j] = 1;
            up[i][j] = up[i - 1][j] + 1;
          } else {
            left[i][j] = left[i][j - 1] + 1;
            up[i][j] = up[i - 1][j] + 1;
          }
        }
      }
    }
    merge(left, up);
    return left;
  }

  private int[][] calculateRightDown(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    int[][] right = new int[m][n];
    int[][] down = new int[m][n];
    for (int i = m - 1; i >= 0; i--) {
      for (int j = n - 1; j >= 0; j--) {
        if (matrix[i][j] == 1) {
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
    merge(right, down);
    return right;
  }

  private int merge(int[][] d1, int[][] d2) {
    int m = d1.length, n = d1[0].length;
    int longestWing = 0;
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        d1[i][j] = Math.min(d1[i][j], d2[i][j]);
        longestWing = Math.max(longestWing, d1[i][j]);
      }
    }
    return longestWing;
  }
}
```

2025 code:

```java
public class Solution {
  public int largest(int[][] matrix) {
    if(matrix.length == 0 || matrix[0].length == 0) {
      return 0;
    }
    return merge(calcLeftUp(matrix), calcRightDown(matrix));
  }

  private int[][] calcLeftUp(int[][] matrix) {
    int m = matrix.length;
    int n = matrix[0].length;
    int[][] left = new int[m][n];
    int[][] up = new int[m][n];

    for(int i = 0; i < m; i++) {
      for(int j = 0; j < n; j++) {
        left[i][j] = matrix[i][j] == 1 ? consecOneLen(left, i, j - 1) + 1 : 0;
        up[i][j] = matrix[i][j] == 1 ? consecOneLen(up, i - 1, j) + 1 : 0;
      }
    }
    merge(left, up);
    return left;
  }

  private int[][] calcRightDown(int[][] matrix) {
    int m = matrix.length;
    int n = matrix[0].length;
    int[][] right = new int[m][n];
    int[][] down = new int[m][n];

    for(int i = m - 1; i >= 0; i--) {
      for(int j = n - 1; j >= 0; j--) {
        right[i][j] = matrix[i][j] == 1 ? consecOneLen(right, i, j + 1) + 1 : 0;
        down[i][j] = matrix[i][j] == 1 ? consecOneLen(down, i + 1, j) + 1 : 0;
      }
    }
    merge(right, down);
    return right;
  }

  private int consecOneLen(int[][] dir, int i, int j) {
    if(i < 0 || i >= dir.length || j < 0 || j >= dir[0].length) {
      return 0;
    }
    return dir[i][j];
  }

  private int merge(int[][] dp1, int[][] dp2) {
    int m = dp1.length;
    int n = dp1[0].length;
    int longestArm = 0;
    for(int i = 0; i < m; i++) {
      for(int j = 0; j < n; j++) {
        dp1[i][j] = Math.min(dp1[i][j], dp2[i][j]);
        longestArm = Math.max(longestArm, dp1[i][j]);
      }
    }
    return longestArm;
  }
}
```