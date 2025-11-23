# Laicode 125. Rotate Matrix

参照[leetcode 48](48-Rotate-Image.md)。 

```java
public class Solution {
  public void rotate(int[][] matrix) {
    int n = matrix.length;
    if(n == 0) {
      return;
    }
    int rowMid = (n + 1) / 2;
    int colMid = n / 2;
    for(int i = 0; i < rowMid; i++) {
      for(int j = 0; j < colMid; j++) {
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