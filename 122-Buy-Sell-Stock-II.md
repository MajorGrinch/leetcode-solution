# 122. Best Time to Buy and Sell Stock II

和 I 差不多，但是可以进行任意次数的交易。那么这里其实就简单了，找到所有的连续升序序列的首位差值就好了。

为啥这个方法可以？比如数据`[1, 8, 2, 15]`那肯定是`1`买`8`卖，`2`再买`15`再卖。数据`[1, 8, 15, 6]`，那肯定是`1`买`15`卖。所以是连续的升序序列的首位差值累积起来就是最大收益。

如何计算所有连续的升序序列首位差值？那就是`price[i + 1] > prices[i]`的时候累加差值就好。

Time complexity: O(n)

Space complexity: O(1)

```java
class Solution {
  public int maxProfit(int[] prices) {
    if (prices.length <= 1) {
      return 0;
    }

    int maxW = 0;
    for (int i = 1; i < prices.length; i++) {
      if (prices[i] > prices[i - 1]) {
        maxW += prices[i] - prices[i - 1];
      }
    }
    return maxW;
  }
}
```