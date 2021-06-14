# 85. Maximal Rectangle

给定一个0/1矩阵，求其中最大的由1构成的矩形面积。

这题可以用动态规划的思想，`leftBoundary[j]`表示目前`matrix[i][j]`这个点目前所在以`i`行为底的矩形的左边界，`rightBoundary[j]`表示这个点目前所在以`i`行为底的矩形的右边界，`height[j]`表示这个点目前所在以`i`行为底的矩形的高度。那么我们可以通过遍历每一行，求出对应的各个参数，然后就可以求出全局最大的矩形面积了。

对于第`i`行来说，每个`leftBoundary[j]`、`rightBoundary[j]`和`height[j]`怎么算？首先我们用`leftOneStart`和`rightOneStart`记录对于当前行的每一个点来说左边连续1的起点和右边连续1的起点。那么对于`leftBoundary[j]`则有两种情况，
+ `matrix[i][j] == '0'`，那么当前点所在以`i`行为底的矩形的左边界必然是在`j`右边，因为已经被这个`0`点截断了。所以我们就把`leftOneStart`更新为`j + 1`，但是这并不代表之后的点就是1，不过至少我们知道连续的1要被截断。同时，我们也要把`leftBoundary[j] = 0`这并不代表它当前的左边界是多少，这么做只是为了确保之后行的`j`点不会因为这个点受影响。
+ `matrix[i][j] == '1'`，那么当前点所在的以`i`行为底的矩形的左边界来源有两个地方，一个是当前点左边连续1的起点，以及向上的所有连续1点他们对应的`leftBoundary`。根据木桶原理，我们需要取最靠右的。

那么对于每行的`rightBoundary[j]`来说，也是一个道理。接着我们来计算`height[j]`，即`matrix[i][j]`这个点目前所在以`i`行为底的矩形高度。同样，分0/1讨论，
+ `matrix[i][j] == '0'`，那么`height[j] = 0`，0点当然需要截断。
+ `matrix[i][j] == '1'`，那么`height[j]++`，如果该点上面有连续1，那么当然高度自增。如果该点上面是0，那么自增后就是1，该点矩形的高度就是1。

每一行算出所有的`leftBoundary[j]`、`rightBoundary[j]`和`height[j]`之后再进行一遍计算，计算以`i`行为底的最大矩形面积并记录全局最大值。当我们遍历完所有行之后，得到的也就是全局最大的由1构成的矩形面积了。

Time complexity: O(M * N)

Space compelxity: O(N)

```java
class Solution {
  public int maximalRectangle(char[][] matrix) {
    if (matrix.length == 0 || matrix[0].length == 0) {
      return 0;
    }

    int m = matrix.length, n = matrix[0].length;
    int[] leftBoundary = new int[n];
    int[] rightBoundary = new int[n];
    int[] height = new int[n];
    Arrays.fill(rightBoundary, n - 1);
    int maxRec = 0;
    for (int i = 0; i < m; i++) {
      int leftOneStart = 0, rightOneStart = n - 1;
      for (int j = 0; j < n; j++) {
        if (matrix[i][j] == '1') {
          height[j]++;
        } else {
          height[j] = 0;
        }
      }

      for (int j = 0; j < n; j++) {
        if (matrix[i][j] == '1') {
          leftBoundary[j] = Math.max(leftBoundary[j], leftOneStart);
        } else {
          leftBoundary[j] = 0;
          leftOneStart = j + 1;
        }
      }

      for (int j = n - 1; j >= 0; j--) {
        if (matrix[i][j] == '1') {
          rightBoundary[j] = Math.min(rightBoundary[j], rightOneStart);
        } else {
          rightBoundary[j] = n - 1;
          rightOneStart = j - 1;
        }
      }

      for (int j = 0; j < n; j++) {
        maxRec = Math.max((rightBoundary[j] - leftBoundary[j] + 1) * height[j], maxRec);
      }
    }

    return maxRec;
  }
}
```