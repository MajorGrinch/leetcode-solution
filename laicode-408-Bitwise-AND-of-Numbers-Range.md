# Laicode 408. Bitwise AND of Numbers Range

这题我们可以思考一下，[m, n]这个区间内所有数字的二进制形式。连续的与之后，低位肯定是变0的，唯一的问题是哪些低位需要变0？

把m和n一直不停向右移位直到相等，移动的位数就是消0的位数。最后的结果再左移回去就是连续与的结果。

```java
public class Solution {
  public int rangeBitwiseAnd(int m, int n) {
    int shift = 0;
    while(m < n) {
      m >>= 1;
      n >>= 1;
      shift++;
    }
    return n << shift;
  }
}
```