# 632. Can I Win

和[laicode 657](laicode-657-Can-I-Win-II.md)类似，只不过这题对手下一步取哪一端是未知的。怎么办？那我们就假设对手会走最优的那一步，也就是说状态转移方程实现的时候，我们这一步不管取哪一端，我们走下一步的收益都要认为是小的那个。

Time complexity: O(N^2)

Space complexity: O(N)

```java
public class Solution {
  public boolean canWin(int[] nums) {
    int n = nums.length;
    int[][] res = new int[n][n];
    int sum = 0;
    for(int i = 0; i < n; i++) {
      res[i][i] = nums[i];
      if(i + 1 < n) {
        res[i][i + 1] = Math.max(nums[i], nums[i + 1]);
      }
      sum += nums[i];
    }
    for(int size = 3; size <= n; size++) {
      for(int i = 0; i + size - 1 < n; i++) {
        int j = i + size - 1;
        int takeLeft = nums[i] + Math.min(res[i + 2][j], res[i + 1][j - 1]);
        int takeRight = nums[j] + Math.min(res[i + 1][j - 1], res[i][j - 2]);
        res[i][j] = Math.max(takeLeft, takeRight);
      }
    }
    return res[0][n - 1] >= sum - res[0][n - 1];
  }
}
```