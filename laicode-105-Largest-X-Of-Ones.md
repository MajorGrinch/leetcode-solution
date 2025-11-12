# Laicode 105. Largest X Of 1s

给定一个01二维数组，求出最大由1组成的`X`形状。其实和十字架那题差不多，只是把十字架斜过来变成了`X`形。

那么这次，我们就预先计算好每个坐标左上，右上，左下，右下四个方向的连续1个数，然后按照和十字架那题一样的方法解决就好。参照[laicode 104](laicode-104-Longest-Cross-Of-1s.md)。

Time complexity: O(M * N)

Space complexity: O(M * N)

```java
public class Solution {
  public int largest(int[][] matrix) {
    if (matrix.length == 0 || matrix[0].length == 0) {
      return 0;
    }
    int[][] upDir = calculateUpDir(matrix);
    int[][] downDir = calculateDownDir(matrix);
    return merge(upDir, downDir);
  }

  private int[][] calculateUpDir(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    int[][] upLeft = new int[m][n], upRight = new int[m][n];
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        if (matrix[i][j] == 1) {
          upLeft[i][j] = getConsecOnes(upLeft, i - 1, j - 1) + 1;
          upRight[i][j] = getConsecOnes(upRight, i - 1, j + 1) + 1;
        }
      }
    }
    merge(upLeft, upRight);
    return upLeft;
  }

  private int[][] calculateDownDir(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    int[][] downLeft = new int[m][n], downRight = new int[m][n];
    for (int i = m - 1; i >= 0; i--) {
      for (int j = n - 1; j >= 0; j--) {
        if (matrix[i][j] == 1) {
          downLeft[i][j] = getConsecOnes(downLeft, i + 1, j - 1) + 1;
          downRight[i][j] = getConsecOnes(downRight, i + 1, j + 1) + 1;
        }
      }
    }
    merge(downLeft, downRight);
    return downLeft;
  }

  private int merge(int[][] m1, int[][] m2) {
    int m = m1.length, n = m1[0].length;
    int maxArm = 0;
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        m1[i][j] = Math.min(m1[i][j], m2[i][j]);
        maxArm = Math.max(maxArm, m1[i][j]);
      }
    }
    return maxArm;
  }

  private int getConsecOnes(int[][] dir, int i, int j) {
    if (i < 0 || i >= dir.length || j < 0 || j >= dir[0].length) {
      return 0;
    }
    return dir[i][j];
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
    return merge(calcUp(matrix), calcDown(matrix));
  }

  private int[][] calcUp(int[][] matrix) {
    int m = matrix.length;
    int n = matrix[0].length;
    int[][] upLeft = new int[m][n];
    int[][] upRight = new int[m][n];

    for(int i = 0; i < m; i++) {
      for(int j = 0; j < n; j++) {
        upLeft[i][j] = matrix[i][j] == 1 ? consecOneLen(upLeft, i - 1, j - 1) + 1 : 0;
        upRight[i][j] = matrix[i][j] == 1 ? consecOneLen(upRight, i - 1, j + 1) + 1 : 0;
      }
    }

    merge(upLeft, upRight);
    return upLeft;
  }

  private int[][] calcDown(int[][] matrix) {
    int m = matrix.length;
    int n = matrix[0].length;
    int[][] downLeft = new int[m][n];
    int[][] downRight = new int[m][n];

    for(int i = m - 1; i >= 0; i--) {
      for(int j = n - 1; j >= 0; j--) {
        downLeft[i][j] = matrix[i][j] == 1 ? consecOneLen(downLeft, i + 1, j - 1) + 1 : 0;
        downRight[i][j] = matrix[i][j] == 1 ? consecOneLen(downRight, i + 1, j + 1) + 1 : 0;
      }
    }

    merge(downLeft, downRight);
    return downLeft;
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