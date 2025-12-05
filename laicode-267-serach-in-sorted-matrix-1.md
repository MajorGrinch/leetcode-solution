# Laicode 267. Search In Sorted Matrix I

参照[leetcode 74](74-Search-a-2D-Matrix.md)。

```java
public class Solution {
  public int[] search(int[][] matrix, int target) {
    if(matrix == null || matrix.length == 0 || matrix[0].length == 0) {
      return new int[]{-1, -1};
    }
    int m = matrix.length;
    int n = matrix[0].length;
    if(target < matrix[0][0] || matrix[m - 1][n - 1] < target) {
      return new int[]{-1, -1};
    }
    int l = 0;
    int r = m - 1;
    int targetRow = -1;
    // find row first
    while(l <= r) {
      int mid = l + (r - l) / 2;
      if(matrix[mid][0] <= target && target <= matrix[mid][n - 1]) {
        targetRow = mid;
        break;
      } else if(target < matrix[mid][0]) {
        r = mid - 1;
      } else {
        // target > matrix[mid][n - 1]
        l = mid + 1;
      }
    }

    // find target in target row
    l = 0;
    r = n - 1;
    while(l <= r) {
      int mid = l + (r - l) / 2;
      if(matrix[targetRow][mid] == target) {
        return new int[] {targetRow, mid};
      } else if(target < matrix[targetRow][mid]) {
        r = mid - 1;
      } else {
        l = mid + 1;
      }
    }
    return new int[] {-1, -1};

  }
}
```