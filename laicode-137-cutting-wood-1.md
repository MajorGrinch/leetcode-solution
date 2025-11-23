# Laicode 137. Cutting Wood I

参照[leetcode 1547](1547-Minimum-Cost-To-Cut-Stick.md)。

```java
public class Solution {
  public int minCost(int[] cuts, int length) {
    int[] pos = new int[cuts.length + 2];
    pos[0] = 0;
    for(int i = 0; i < cuts.length; i++) {
      pos[i + 1] = cuts[i];
    }
    pos[pos.length - 1] = length;
    int n = pos.length;
    int[][] cost = new int[n][n];
    for(int size = 2; size <= n - 1; size++) {
      for(int i = 0; i < n - size; i++) {
        int minCost = Integer.MAX_VALUE;
        for(int j = i + 1; j < i + size; j++) {
          minCost = Math.min(minCost, cost[i][j] + cost[j][i + size] + pos[i + size] - pos[i]);
        }
        cost[i][i + size] = minCost;
      }
    }
    return cost[0][n - 1];
  }
}
```