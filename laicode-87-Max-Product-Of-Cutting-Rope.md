# Laicode 87. Max Product Of Cutting Rope

给定一个长度为`length`的绳子，将其切割为若干段，使得各段长度乘积最大。

经典DP题。我们用`maxProduct[i]`来表示长度为`i`的绳子的最大切割乘积。那么`maxProduct[i]`就有两类切法，
+ 只切一刀，在x处切一刀乘积`x * (i - x)`是最大的。
+ 切多刀，切多刀我们可转化为其中x长度切多刀，剩下来(i-x)额外一刀。那么就是`maxProduct[x] * (i - x)`最大。

根据以上思路，对于每一个子问题`maxProduct[i]`，我们都需要遍历所有它所有的子问题以求出它的答案。所以双重循环，来解决所有子问题，最后就可以得到length的最大切割乘积。

Time complexity: O(N^2)

Space complexity: O(N)

```java
public class Solution {
  public int maxProduct(int length) {
    int[] maxProduct = new int[length + 1];
    maxProduct[1] = 1;
    for (int i = 2; i <= length; i++) {
      int localMax = 0;
      for (int j = 1; j < i; j++) {
        localMax = Math.max(localMax, Math.max(maxProduct[j], j) * (i - j));
      }
      maxProduct[i] = localMax;
    }
    return maxProduct[length];
  }
}
```