# 48. Rotate Image

给定一个N*N的正方形矩阵，顺时针旋转90度。

这题需要注意的是N的奇偶，当N是偶数的时候很好办，矩形被分成四个小正方形然后直接旋转就行。当N是奇数的时候，我们只能分成四个矩形然后旋转。

所以计算rowMid的时候，我们用`(n + 1) / 2`来计算。这样当n是偶数时，还是一半。当n时奇数时，n就可以多算一行。colMid严格遵守`n / 2`就可以，偶数时候是一般的列，奇数时候就可以少算中间那列。因为中间那列用来给右边的矩形了。

Time complexity: O(N * N)

Space complexity: O(1)

```java
class Solution {
  public void rotate(int[][] matrix) {
    if (matrix.length <= 1) return;
    int n = matrix.length;
    int rowMid = (n + 1) / 2;
    int colMid = n / 2;
    for (int i = 0; i < rowMid; i++) {
      for (int j = 0; j < colMid; j++) {
        int temp = matrix[i][j];
        matrix[i][j] = matrix[n - 1 - j][i];
        matrix[n - 1 - j][i] = matrix[n - 1 - i][n - 1 - j];
        matrix[n - 1 - i][n - 1 - j] = matrix[j][n - 1 - i];
        matrix[j][n - 1 - i] = temp;
      }
    }
  }
}
```