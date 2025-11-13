# Laicode 637. Largest Square Surrounded By One

参照[leetcode 1139](1139-Largest-1-Bordered-Square.md)。

```java
public class Solution {
  public int largestSquareSurroundedByOne(int[][] matrix) {
    if(matrix == null || matrix.length == 0 || matrix[0].length == 0) {
      return 0;
    }
    List<int[][]> consecOneList = calcRightAndDown(matrix);
    int[][] rightOne = consecOneList.get(0);
    int[][] downOne = consecOneList.get(1);
    int m = matrix.length;
    int n = matrix[0].length;
    int res = 0;
    for(int i = 0; i < m; i++) {
      for(int j = 0; j < n; j++) {
        if(matrix[i][j] != 1) {
          continue;
        }
        int len = Math.min(rightOne[i][j], downOne[i][j]);
        for(int k = len; k > 0; k--) {
          if(rightOne[i + k - 1][j] >= k && downOne[i][j + k - 1] >= k) {
            res = Math.max(res, k);
            break;
          }
        }
      }
    }
    return res;
  }

  private List<int[][]> calcRightAndDown(int[][] matrix) {
    int m = matrix.length;
    int n = matrix[0].length;
    int[][] rightOne = new int[m][n];
    int[][] downOne = new int[m][n];
    for(int i = m - 1; i >= 0; i--) {
      for(int j = n - 1; j >= 0; j--) {
        rightOne[i][j] = matrix[i][j] == 1 ? consecOneLen(rightOne, i, j + 1) + 1 : 0;
        downOne[i][j] = matrix[i][j] == 1 ? consecOneLen(downOne, i + 1, j) + 1 : 0;
      }
    }
    return Arrays.asList(rightOne, downOne);
  }

  private int consecOneLen(int[][] dp, int i, int j) {
    if(i < 0 || i >= dp.length || j < 0 || j >= dp[0].length) {
      return 0;
    }
    return dp[i][j];
  }
}
```