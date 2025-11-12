# 221. Maximal Square

给定一个0/1矩阵，求出里面最大全1正方形的面积。

经典DP题，用maxSq数组来存放子问题的答案，`maxSq[i][j]`表示以`matrix[i][j]`为右下角的全1正方形最大边长。那么`matrix[i][j]`如果为0的话，边长就是0。如果`matrix[i][j]`为1的话，`matrix[i][j]`就来自于`[i - 1][j - 1]`，`[i - 1][j]`和`[i][j - 1]`里面最小的全1正方形边长再加1。但是如果`i`和`j`有一个为0，那么`maxSq[i][j]`就取决于`matrix[i][j]`是否为1。

如此一来，我们可以算出以每个`[i][j]`为右下角的最大全1正方形周长，我们就可以得到整个矩阵里最大的全1正方形的边长，从而计算出面积。

Time complexity: O(M * N).

Space complexity: O(M * N).

```java
class Solution {
  public int maximalSquare(char[][] matrix) {
    if (matrix.length == 0 || matrix[0].length == 0) {
      return 0;
    }
    int m = matrix.length, n = matrix[0].length;
    int[][] maxSq = new int[m][n];
    int maxSize = 0;
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        if (i == 0 || j == 0) {
          maxSq[i][j] = matrix[i][j] == '1' ? 1 : 0;
        } else if (matrix[i][j] == '1') {
          maxSq[i][j] =
              Math.min(maxSq[i - 1][j - 1], Math.min(maxSq[i - 1][j], maxSq[i][j - 1])) + 1;
        }
        maxSize = Math.max(maxSize, maxSq[i][j]);
      }
    }
    return maxSize * maxSize;
  }
}
```

2025 code:

```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        int M = matrix.length;
        int N = matrix[0].length;
        int[][] sq = new int[M][N];
        int res = 0;
        for(int i = 0; i < M; i++) {
            sq[i][0] = matrix[i][0] - '0';
            res = Math.max(res, sq[i][0]);
        }
        for(int j = 0; j < N; j++) {
            sq[0][j] = matrix[0][j] - '0';
            res = Math.max(res, sq[0][j]);
        }
        for(int i = 1; i < M; i++) {
            for(int j = 1; j < N; j++) {
                if(matrix[i][j] == '1') {
                    sq[i][j] = Math.min(sq[i - 1][j - 1], Math.min(sq[i - 1][j], sq[i][j - 1])) + 1;
                    res = Math.max(res, sq[i][j]);
                }
            }
        }
        return res * res;
    }
}
```