# Laicode 87. Max Product Of Cutting Rope

给定一个长度为`length`的绳子，将其切割为若干段（至少 2 段），使得各段长度乘积最大。

经典DP题。我们用`maxProduct[i]`来表示长度为`i`的绳子的最大切割乘积。那么`maxProduct[i]`就可以表示为`x`和`i - x`这两段。这两段内部可以继续切，也可以不切了，无论哪种只要最后乘积最大就行。

那么对于任意的 i，就需要遍历`x = [1, i/2]`的情况，每个x 的对应最大乘积就是 `Math.max(x, dp[x]) * Math.max(i - x, dp[i - x])`。

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

2025 code:

```java
public class Solution {
  public int maxProduct(int length) {
    int[] maxProduct = new int[length + 1];
    for(int i = 2; i <= length; i++) {
      for(int j = 1; j <= i / 2; j++) {
        int temp = Math.max(j, maxProduct[j]) * Math.max(i - j, maxProduct[i - j]);
        maxProduct[i] = Math.max(maxProduct[i], temp);
      }
    }
    return maxProduct[length];
  }
}
```