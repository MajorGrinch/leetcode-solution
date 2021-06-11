# Laicode 96. Merge Stones

和[1547. Minimum Cost to Cut a Stick](1547-Minimum-Cost-To-Cut-Stick.md)差不多，这里是合并石头。

那么还是动态规划的思想，`cost[i][j]`表示`stones[i, j]`合并为一个石头所需的代价。那么有状态转移方程`cost[i][j] = MIN(cost[i][k] + cost[k + 1][j]) + stones[i, j], i <= k < j`，因为`stones[i, j]`肯定要先合并到只剩两个石头，再合并为一个石头。`MIN(cost[i][k] + cost[k + 1][j])`就旨在求出合并到剩两个石头的最小代价，这俩石头合并的代价肯定是`stones[i, j]`的总重量，故有此状态转移方程。

这个状态转移方程就要求我们要尽量求出`i`和`j`间隔较小的`cost[i][j]`，再慢慢扩大。所以，我们`size`从2遍历到`n`，1不用管因为肯定是石头自重。求出每个size对应的每个`cost[i][i + size - 1]`，以供之后更大的`size`调用。最后`cost[0][n - 1]`就是合并所有石头的最小代价。

Time complexity: O(n^3)

Space complexity: O(n^2)

```java
public class Solution {
  public int minCost(int[] stones) {
    int n = stones.length;
    if (n == 1) {
      return 0;
    }

    int[][] cost = new int[n][n];
    for (int size = 2; size <= n; size++) {
      for (int i = 0; i <= n - size; i++) {
        int j = i + size - 1;
        int iSzieCost = Integer.MAX_VALUE;
        for (int k = i; k < j; k++) {
          iSzieCost = Math.min(iSzieCost, cost[i][k] + cost[k + 1][j]);
        }
        for (int k = i; k <= j; k++) {
          iSzieCost += stones[k];
        }
        cost[i][j] = iSzieCost;
      }
    }
    return cost[0][n - 1];
  }
}
```