# 1547. Minimum Cost to Cut a Stick

给定一个长度为`len`的木棍和对应的一个数组存放切割点，切割一段木棍的费用是当前木棍的长度。求出按照切割点切割掉木棍所需的最小费用。

这题运用动态规划的思想，我们先把切割点扩充首尾各自加上`0`和`len`，这样我们就凑齐了切割结束之后所有木棍的所有端点存在`positions`数组里。`cost[i][j]`表示`position[i]`到`position[j]`的木棍切割到不可分为止的费用，那么我们有状态转移方程`cost[i][j] = MIN(cost[i][k] + cost[k][j]) + position[j] - position[i],  i < k < j`。那么显然整根木棍的切割费用就是`cost[0][positions.length - 1]`。

那么怎么把大问题分解成子问题，然后由子问题的答案得到大问题的答案？由状态转移方程我们可以得知，端点`i`到`j`组成的子段切割由最小的`cost[i][k] + cost[k][j]`和本段长度组成。可是`cost[i][k]`和`cost[k][j]`怎么得到的？我们可以知道，`[i][k]`和`[k][j]`肯定是比`[i][j]`短的，所以也就是说我们要先求出比当前`[i][j]`要短的子段费用，然后才能得到`[i][j]`的费用。所以我们针对所有子段个数从`2`到`n - 1`进行遍历，求出以每个下标开始的各个段数的子段切割费用，这样我们就可以最后得到`n-1`段的切割费用。

Time complexity: O(n^3)

Space complexity: O(n^2)

```java
class Solution {
  public int minCost(int len, int[] cuts) {
    Arrays.sort(cuts);
    int n = cuts.length;
    int[] positions = new int[n + 2];
    positions[0] = 0;
    for (int i = 0; i < cuts.length; i++) {
      positions[i + 1] = cuts[i];
    }
    positions[n + 1] = len;
    n = positions.length;

    int[][] cost = new int[n][n];
    for (int size = 2; size <= n - 1; size++) {
      for (int i = 0; i <= n - 1 - size; i++) {
        int iSizeFutureCost = Integer.MAX_VALUE;
        for (int j = i + 1; j < i + size; j++) {
          iSizeFutureCost = Math.min(iSizeFutureCost, cost[i][j] + cost[j][i + size]);
        }
        cost[i][i + size] = iSizeFutureCost + positions[i + size] - positions[i];
      }
    }
    return cost[0][n - 1];
  }
}
```