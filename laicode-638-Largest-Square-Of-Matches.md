# Laicode 638. Largest Square Of Matches

给定一个矩阵，每个元素由0123组成。0表示该坐标周围没有火柴，1代表右边有一个火柴，2代表下面有一个火柴，3代表右边和下面都有火柴。求出这个矩阵里最大的由火柴组成的正方形。

这题和[leetcode 1139](1139-Largest-1-Bordered-Square.md)很像，但是有略微的区别。因为火柴只会放在一个坐标的右方和下方。我们还是一个思路，预计算出给定的每个坐标他们右边和下面有多少个连续的火柴。

预计算只会，遍历整个矩阵。对于每个右方和下方都有火柴的坐标，我们再去看看以它为左上角的最大正方形是多少。计算方法如出一辙，先取出右方和下方火柴长度中小的那个，然后对于所有的`0 <= k <= edge`找到最大的满足`right[i + k][j] >= k`和`down[i][j + k] >= k`的`k`。

Time complexity: O(M * N)

Space complexity: O(M * N)

```java
public class Solution {
  public int largestSquareOfMatches(int[][] matrix) {
    if (matrix.length == 0 || matrix[0].length == 0) {
      return 0;
    }
    int m = matrix.length, n = matrix[0].length;
    List<int[][]> downRight = calculateDownRight(matrix);
    int[][] down = downRight.get(0);
    int[][] right = downRight.get(1);
    int maxSq = 0;
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        if (matrix[i][j] == 3) {
          int edge = Math.min(down[i][j], right[i][j]);
          for (int k = edge; k >= 0; k--) {
            if (right[i + k][j] >= k && down[i][j + k] >= k) {
              edge = k;
              break;
            }
          }
          maxSq = Math.max(maxSq, edge);
        }
      }
    }
    return maxSq;
  }

  private List<int[][]> calculateDownRight(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    int[][] right = new int[m][n];
    int[][] down = new int[m][n];
    for (int i = m - 1; i >= 0; i--) {
      for (int j = n - 1; j >= 0; j--) {
        if ((matrix[i][j] & 1) > 0) {
          right[i][j] = getConsecOnes(right, i, j + 1) + 1;
        }
        if ((matrix[i][j] & 2) > 0) {
          down[i][j] = getConsecOnes(down, i + 1, j) + 1;
        }
      }
    }
    List<int[][]> downRight = new ArrayList<>();
    downRight.add(down);
    downRight.add(right);
    return downRight;
  }

  private int getConsecOnes(int[][] dir, int i, int j) {
    if (i < 0 || i >= dir.length || j < 0 || j >= dir[0].length) {
      return 0;
    }
    return dir[i][j];
  }
}
```