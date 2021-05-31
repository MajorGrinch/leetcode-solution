# Laicode 657. Can I Win II

和朋友van♂游戏，给定一个数组两人轮流从两头开始拿数，最后谁的分数多谁就赢。朋友不太聪明的亚子，只会从两头中拿大的那个数。而你则会三思而且你先手，求出你最后的最高成绩能是多少。

没想到吧，这题也能动态规划。`dp[i][j]`表示`nums[i,j]`这个子数组让我先手，我最高成绩可以多少分。那么不可分割的子问题就有两个情况，
+ `dp[i][i] = nums[i]`，这个不难理解，就一个数让我拿。
+ `dp[i][i + 1] = Math.max(nums[i], nums[i + 1])`，这个也好说，两个数的数组，那我肯定拿大的那个。

Base case解决了，那我们就得利用base case去解决一般情况。当子数组的长度不再是1和2，而是3、4直到`n`的时候，我们就可以慢慢求出题目给定的数组我能拿多少分。`dp[i][j]`怎么算？还是分情况讨论：
+ 我们拿`nums[i]`，那么朋友就只有`nums[i + 1]`和`nums[j]`可以选而且他只会选大的。如果`nums[i + 1]`比较大，那么`dp[i][j] = nums[i] + dp[i + 2][j]`。否则，`dp[i][j] = nums[i] + dp[i + 1][j - 1]`。
+ 我们拿`nums[j]`，那么朋友只有`nums[i]`和`nums[j - 1]`可以选而且他会选大的。如果`nums[i]`比较大，那么`dp[i][j] = nums[j] + dp[i + 1][j]`。否则，`dp[i][j] = nums[j] + dp[i][j - 2]`。

那么对于这两种情况，我们肯定是选择收益大的那个给`dp[i][j]`。具体到代码实现上，size为1和2的base case我们可以轻松得到。size从3遍历到n，针对每个下标作为开始的`size`长度子数组进行计算，即`[i, i + size - 1]`。注意`i`的上界就好。

最后`dp[0][n-1]`就是整个数组让我先手，我可以得多少分。

```java
public class Solution {
  public int canWin(int[] nums) {
    int n = nums.length;
    int[][] dp = new int[n][n];
    for (int i = 0; i < n; i++) dp[i][i] = nums[i];
    for (int i = 0; i < n - 1; i++) dp[i][i + 1] = Math.max(nums[i], nums[i + 1]);
    for (int size = 3; size <= n; size++) {
      for (int i = 0; i <= n - size; i++) {
        int j = i + size - 1;
        int takeLeft = nums[i] + (nums[i + 1] > nums[j] ? dp[i + 2][j] : dp[i + 1][j - 1]);
        int takeRight = nums[j] + (nums[i] > nums[j - 1] ? dp[i + 1][j - 1] : dp[i][j - 2]);
        dp[i][j] = Math.max(takeLeft, takeRight);
      }
    }
    return dp[0][n - 1];
  }
}
```