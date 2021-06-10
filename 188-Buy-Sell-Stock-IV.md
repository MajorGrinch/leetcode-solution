# 188. Best Time to Buy and Sell Stock IV

给定一个数组的股价，最多允许k次买卖，求出最大收益。

这题可以用动态规划的思想来解决，`profit[i][j]`代表`prices[0, j]`里最多`i`次买卖产生的最大收益。那么自然，`i`要`1 -> k`，`j`要`1 -> n - 1`。为啥`j`要从`1`开始？完成一次交易肯定需要至少两个点吧。

我们计算`profit[i][j]`的时候，对应计算`prices[0, j]`里最多`i`次买卖产生的最大收益。计算的时候，无非两种情况`j`天卖or不卖。`j`天不卖的话，那么`profit[i][j] = profit[i][j - 1]`，也就是在`prices[0, j - 1]`里完成`i`次买卖的收益。`j`天卖的话，那么就得获得`prices[0, j - 1]`里完成`i - 1`次买卖并且再买入一次的收益，加上`prices[j]`。这两种情况，哪个获益多，`profit[i][j]`就等于谁。

为了计算第二种情况，对于每个`j`，我们都需要知道`prices[0, j - 1]`里完成`i - 1`次买卖并且再买入一次的收益。那么显然，它的值等于`MAX(profit[i - 1][k - 1] - prices[k]), k < j`。这意味着我们要再多一层循环吗？是，也不是。我们当然可以通过多一层循环的方式来计算这个值，但是我们也可以通过累积的方式来记录。观察一下以下代码

```java
int val = Integer.MAX_VALUE;
for(int k = 1; k < j; k++){
  val = Math.max(val, profit[i - 1][k - 1] - prices[k])
}
```

随着`j`从`1`到`n - 1`遍历，以上代码其实包含了大量重复计算。想想看，我们其实想要的只是当前`j`之前的最大的`profit[i - 1][k - 1] - prices[k]`。我们用`currProfit`来记录。当前的`currProfit`有两个来源，要么还是自己，要么是`profit[i - 1][j - 2] - prices[j - 1]`，这两者取较大的。`currProfit`初始化为`-prices[0]`因为我们至少要有一次卖出的操作。当前`j`计算出`currProfit`之后留给下一轮`j + 1`使用，也就对应着`prices[0, j]`里面进行`i - 1`次买卖和再多一次卖出的最大收益。

Time complexity: O(k * n)

Space complexity: O(k * n)

```java
class Solution {
  public int maxProfit(int k, int[] prices) {
    if (k == 0 || prices.length <= 1) {
      return 0;
    }

    int n = prices.length;
    if (k >= n / 2) {
      int maxW = 0;
      for (int i = 1; i < n; i++) {
        if (prices[i] > prices[i - 1]) {
          maxW += prices[i] - prices[i - 1];
        }
      }
      return maxW;
    }

    int[][] profit = new int[k + 1][n];
    for (int i = 1; i <= k; i++) {
      int currProfit = -prices[0];
      for (int j = 1; j < n; j++) {
        profit[i][j] = Math.max(profit[i][j - 1], currProfit + prices[j]);
        currProfit = Math.max(currProfit, profit[i - 1][j - 1] - prices[j]);
      }
    }
    return profit[k][n - 1];
  }
}
```