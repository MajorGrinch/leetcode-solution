# Laicode 101. Largest Square Of 1s

参照[leetcode 221](221-Maximal-Square.md)。

```java
public class Solution {
  public int largest(int[][] matrix) {
    int N = matrix.length;
    int[][] square = new int[N][N];
    int res = 0;
    for(int i = 0; i < N; i++) {
      square[i][0] = matrix[i][0];
      res = Math.max(res, square[i][0]);
    }
    for(int j = 0; j < N; j++) {
      square[0][j] = matrix[0][j];
      res = Math.max(res, square[0][j]);
    }
    for(int i = 1; i < N; i++) {
      for(int j = 1; j < N; j++) {
        if(matrix[i][j] == 1) {
          square[i][j] = Math.min(square[i - 1][j - 1], Math.min(square[i - 1][j], square[i][j - 1])) + 1;
          res = Math.max(res, square[i][j]);
        }
      }
    }
    return res;
  }
}
```