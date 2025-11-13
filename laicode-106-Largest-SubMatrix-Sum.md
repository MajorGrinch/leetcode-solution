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

2025 update:

我把 colPrefixSum稍微改了一下定义，`colPrefixSum[i][j]` 表示第 j 列的 `[0, i)` 行的前缀和。那么很显然，colPrefixSum 数组会有 `m + 1`行。在遍历 top 和 bottom 的时候，我们把`[bottom, top)`行进行压缩，从而更加直观地搭配。`0 <= bottom <= m - 1`，`bottom + 1 <= top <= m`。

```java
public class Solution {
  public int largest(int[][] matrix) {
    if(matrix == null || matrix.length == 0 || matrix[0].length == 0) {
      return 0;
    }
    int[][] colPrefixSum = getColPrefixSum(matrix);
    int m = matrix.length;
    int n = matrix[0].length;
    int res = Integer.MIN_VALUE;
    // [bottom, top)
    for(int bottom = 0; bottom < m; bottom++) {
      for(int top = bottom + 1; top < m + 1; top++) {
        int[] compress = new int[n];
        for(int j = 0; j < n; j++) {
          compress[j] = colPrefixSum[top][j] - colPrefixSum[bottom][j];
        }
        int localMax = maxSubArraySum(compress);
        res = Math.max(res, localMax);
      }
    }
    return res;
  }

  private int maxSubArraySum(int[] array) {
    int curr = array[0];
    int res = curr;
    for(int i = 1; i < array.length; i++) {
      if(curr > 0) {
        curr += array[i];
      } else {
        curr = array[i];
      }
      res = Math.max(res, curr);
    }
    return res;
  }

  private int[][] getColPrefixSum(int[][] matrix) {
    int m = matrix.length;
    int n = matrix[0].length;
    int[][] colPreSum = new int[m + 1][n];
    for(int j = 0; j < n; j++) {
      for(int i = 1; i < m + 1; i++) {
        colPreSum[i][j] = colPreSum[i - 1][j] + matrix[i - 1][j];
      }
    }
    return colPreSum;
  }
}
```