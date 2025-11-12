# Laicode 100. Edit Distance

参照[leetcode 72](72-Edit-Distance.md)。

```java
public class Solution {
  public int editDistance(String one, String two) {
    int m = one.length();
    int n = two.length();
    int[][] distance = new int[m + 1][n + 1];
    for(int i = 0; i < m + 1; i++) {
      distance[i][0] = i;
    }
    for(int j = 0; j < n + 1; j++) {
      distance[0][j] = j;
    }
    for(int i = 1; i < m + 1; i++) {
      for(int j = 1; j < n + 1; j++) {
        int replace = one.charAt(i - 1) == two.charAt(j - 1) ? distance[i - 1][j - 1] : distance[i - 1][j - 1] + 1;
        int delete = distance[i - 1][j] + 1;
        int insert = distance[i][j - 1] + 1;
        distance[i][j] = Math.min(replace, Math.min(delete, insert));
      }
    }
    return distance[m][n];
  }
}
```