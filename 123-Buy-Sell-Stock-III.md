# 123. Best Time to Buy and Sell Stock III

和[188. Best Time to Buy and Sell Stock IV](188-Buy-Sell-Stock-IV.md)差不多，那个是最多k次交易的通解，看那个去。

```java
class Solution {
  public int maxProfit(int[] prices) {
    return maxProfit(2, prices);
  }

  private int maxProfit(int k, int[] prices) {
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