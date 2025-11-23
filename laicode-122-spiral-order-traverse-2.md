# Laicode 121. Spiral Order Traverse II

参照[leetcode 54](54-Spiral-Matrix.md)。

```java
public class Solution {
  public List<Integer> spiral(int[][] matrix) {
    List<Integer> res = new ArrayList<>();
    if(matrix.length == 0 || matrix[0].length == 0){
      return res;
    }
    int m = matrix.length, n = matrix[0].length;
    helper(matrix, 0, m - 1, 0, n - 1, res);
    return res;
  }

  private void helper(int[][] matrix, int rowS, int rowE, int colS, int colE, List<Integer> res) {
    if(rowS > rowE || colS > colE) {
      return;
    }
    if(rowS == rowE) {
      for(int i = colS; i <= colE; i++) {
        res.add(matrix[rowS][i]);
      }
    } else if(colS == colE) {
      for(int i = rowS; i <= rowE; i++) {
        res.add(matrix[i][colS]);
      }
    } else {
      for(int i = colS; i < colE; i++) {
        res.add(matrix[rowS][i]);
      }
      for(int i = rowS; i < rowE; i++) {
        res.add(matrix[i][colE]);
      }
      for(int i = colE; i > colS; i--) {
        res.add(matrix[rowE][i]);
      }
      for(int i = rowE; i > rowS; i--) {
        res.add(matrix[i][colS]);
      }
      helper(matrix, rowS + 1, rowE - 1, colS + 1, colE - 1, res);
    }
  }
}
```