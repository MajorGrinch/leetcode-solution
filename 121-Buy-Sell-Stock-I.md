# 121. Best Time to Buy and Sell Stock

给定一个数组代表股票价格，只许买卖最多一次，问最大收益是多少。挣不成就返回0。

## Dynamic Programming Approach

用动态规划的思想，我们记录每个下标之前的最低价格`minPriceBf[i]`，这样通过`prices[i] - minPriceBf[i]`就可以知道第`i`天卖出的收益，最后知道全局最大收益。

那么`minPriceBf[i]`肯定是来自于`minPriceBf[i - 1]`，但是有个问题，如果`i`之前价格都比`i`高该怎么办？这题因为说挣不到钱就返回0，所以在具体实现的时候如果`i`之前价格都比它高的话，`minPriceBf[i] = prices[i]`。这样我们在算第`i`天卖出能获得的收益就是0，不影响结果，也因此`minPriceBf[i]`在具体实现中实际记录的是`[0, i]`股票的最低价格。那么显然，`minPriceBf[i] = MIN(minPriceBf[i - 1], prices[i])`。

Time complexity: O(n)

Space complexity: O(n)

```java
class Solution {
  public int maxProfit(int[] prices) {
    if (prices.length <= 1) {
      return 0;
    }
    int n = prices.length;
    int[] minPriceBf = new int[n];

    int maxW = 0; // shi bu shi xiang zheng W le ?
    minPriceBf[0] = prices[0];
    for (int i = 1; i < n; i++) {
      minPriceBf[i] = Math.min(minPriceBf[i - 1], prices[i]);
      maxW = Math.max(maxW, prices[i] - minPriceBf[i]);
    }
    return maxW;
  }
}
```

## Greedy Algo Approach

我们仔细观察动态规划的思路，我们可以发现我们在`i`的时候需要的只是`[0, i]`的股票最低价格，以获取这个子问题的答案。这些子问题的答案也称局部最优解，最大收益也就是全局最优解最后就出自于这众多的局部最优解中。要素察觉！如此以来，贪心思想就可以派上用场了！~~算法导论，yyds~~

`minPriceBf`不在是一个数组，而只是一个变量，它代表了`[0, i]`的股票最低价格。如果当前`i`价格比它更低，就更新。当前`i`卖出的最大收益也就是`prices[i] - minPricesBf`。

Time complexity: O(n)

Space complexity: O(1)

```java
class Solution {
  public int maxProfit(int[] prices) {
    if (prices.length <= 1) {
      return 0;
    }
    int n = prices.length;

    int minPriceBf = prices[0];
    int maxW = 0; // shi bu shi xiang zheng W le ?
    for (int i = 1; i < n; i++) {
      minPriceBf = Math.min(minPriceBf, prices[i]);
      maxW = Math.max(maxW, prices[i] - minPriceBf);
    }
    return maxW;
  }
}
```

贪心思想的另一种实现方法，实测在LeetCode里跑得比较块。应该是因为总操作数降下来了。

```java
class Solution {
  public int maxProfit(int[] prices) {
    if (prices.length <= 1) {
      return 0;
    }
    int n = prices.length;

    int minPriceBf = prices[0];
    int maxW = 0; // shi bu shi xiang zheng W le ?
    for (int i = 1; i < n; i++) {
      if (minPriceBf < prices[i]) {
        maxW = Math.max(maxW, prices[i] - minPriceBf);
      } else {
        minPriceBf = prices[i];
      }
    }
    return maxW;
  }
}
```