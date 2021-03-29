# Laicode 106. Largest SubMatrix Sum

给定一个二维数组，找出和最大的子矩阵。

这题可算是非常经典的动态规划题了，难度比较大。我们可以针对每一列来计算他们的prefix sum，这样我们就可以快速计算指定列的任意行范围的和，即`a[top][j] + a[top + 1][j] + ... + a[bottom][j]`。

有了这个之后，我们可以用双重for循环遍历所有的top行和bottom行的搭配。对于每个搭配，我们求出每一列对应的`[top, bottom]`行的和，得到一个一维数组。这样我们只要求出这个一维数组的最大子数组和，就可以知道对于top行和bottom行夹着的部分，最大子矩阵和是多少。等我们遍历的所有的`top`和`bottom`搭配之后，我们就知道整个矩阵里最大子矩阵和是多少了。

Time complexity: O(M * M * N)

Space complexity: O(M * N)

```java
public class Solution {
  public int largest(int[][] matrix) {
    if (matrix.length == 0 || matrix[0].length == 0) {
      return 0;
    }
    int m = matrix.length, n = matrix[0].length;
    int[][] colPrefixSum = calculateColPrefixSum(matrix);
    int res = Integer.MIN_VALUE;
    for (int top = 0; top < m; top++) {
      for (int bottom = top; bottom < m; bottom++) {
        int[] colSum = new int[n];
        for (int j = 0; j < n; j++) {
          colSum[j] = colPrefixSum[bottom][j] - (top == 0 ? 0 : colPrefixSum[top - 1][j]);
        }
        int localMaxSum = maxSubarraySum(colSum);
        res = Math.max(res, localMaxSum);
      }
    }
    return res;
  }

  private int maxSubarraySum(int[] array) {
    int currSum = array[0];
    int res = currSum;
    for (int i = 1; i < array.length; i++) {
      if (currSum < 0) {
        currSum = 0;
      }
      currSum += array[i];
      res = Math.max(res, currSum);
    }
    return res;
  }

  private int[][] calculateColPrefixSum(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    int[][] prefixSum = new int[m][n];
    for (int j = 0; j < n; j++) {
      prefixSum[0][j] = matrix[0][j];
    }
    for (int i = 1; i < m; i++) {
      for (int j = 0; j < n; j++) {
        prefixSum[i][j] = prefixSum[i - 1][j] + matrix[i][j];
      }
    }
    return prefixSum;
  }
}
```