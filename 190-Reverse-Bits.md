# 190. Reverse Bits

给定一个int整数，把它的二进制位给翻转过来。

首先我们先实现一个方法，给定一个int，对调i位和j位。那么自然先是获得各自对应的数，如果相同的话，那就没必要对调了。如果不同的话，那么就各自和1进行异或，即可实现对调。

接着，int总共32位，从两边往中间一起对调就可以实现整体反转。

```java
public class Solution {
  // you need treat n as an unsigned value
  public int reverseBits(int n) {
    int i = 0, j = 31;
    while (i < j) {
      n = swapBit(n, i++, j--);
    }
    return n;
  }

  private int swapBit(int n, int i, int j) {
    int iBit = (n >> i) & 1;
    int jBit = (n >> j) & 1;
    if (iBit == jBit) {
      return n;
    }
    int mask = ((1 << i) | (1 << j));
    return n ^ mask;
  }
}
```

Time complexity: O(1)

Space complexity: O(1)