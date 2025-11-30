# Laicode 177. Longest Common Subsequence

参照[leetcode 1143](1143-Longest-Common-Subsequence.md)。

```java
public class Solution {
  public int longest(String source, String target) {
    int m = source.length();
    int n = target.length();
    int[][] lcs = new int[m + 1][n + 1];
    for(int i = 1; i <= m; i++) {
      for(int j = 1; j <= n; j++) {
        if(source.charAt(i - 1) == target.charAt(j - 1)) {
          lcs[i][j] = lcs[i - 1][j - 1] + 1;
        } else {
          lcs[i][j] = Math.max(lcs[i - 1][j], lcs[i][j - 1]);
        }
      }
    }
    return lcs[m][n];
  }
}
```