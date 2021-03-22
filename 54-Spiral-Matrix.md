# 54. Spiral Matrix

## Recursive Approach

给定一个`m * n`的矩阵，将它的元素旋转输出。

那么我们先实现一个辅助方法，给定一个矩阵以及边界范围，旋转输出最外层的数字。这并不难，只要控制好四个循环的边界，就可以将最外层旋转输出。

接着我们就将矩阵的行列边界向内收缩，一层一层的剥开矩阵就可以得到想要的序列了。

Time complexity: O(n)

Space complexity: O(min(m, n)), because of the call stack.

```java
class Solution {
  public List<Integer> spiralOrder(int[][] matrix) {
    List<Integer> res = new ArrayList<>();
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
      return res;
    }
    spiralOrder(matrix, 0, matrix.length - 1, 0, matrix[0].length - 1, res);
    return res;
  }

  private void spiralOrder(
      int[][] matrix, int rStart, int rEnd, int cStart, int cEnd, List<Integer> res) {
    if (rStart > rEnd || cStart > cEnd) {
      return;
    } else if (rStart == rEnd) {
      for (int i = cStart; i <= cEnd; i++) {
        res.add(matrix[rStart][i]);
      }
    } else if (cStart == cEnd) {
      for (int i = rStart; i <= rEnd; i++) {
        res.add(matrix[i][cStart]);
      }
    } else {
      for (int i = cStart; i < cEnd; i++) {
        res.add(matrix[rStart][i]);
      }
      for (int j = rStart; j < rEnd; j++) {
        res.add(matrix[j][cEnd]);
      }
      for (int i = cEnd; i > cStart; i--) {
        res.add(matrix[rEnd][i]);
      }
      for (int j = rEnd; j > rStart; j--) {
        res.add(matrix[j][cStart]);
      }
      spiralOrder(matrix, rStart + 1, rEnd - 1, cStart + 1, cEnd - 1, res);
    }
  }
}
```