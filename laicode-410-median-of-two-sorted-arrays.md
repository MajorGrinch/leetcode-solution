# Laicode 410. Median of Two Sorted Arrays

参照[leetcode 4](4-Median-Of-Two-Sorted-Arrays.md)。

```java
public class Solution {
  public double median(int[] a, int[] b) {
    int len = a.length + b.length;
    if(len % 2 == 1) {
      return kth(a, b, len / 2 + 1, 0, 0);
    } else {
      int med1 = kth(a, b, len / 2, 0, 0);
      int med2 = kth(a, b, len / 2 + 1, 0, 0);
      return (med1 + med2) / 2.0;
    }
  }

  private int kth(int[] a, int[] b, int k, int aStart, int bStart) {
    if(aStart >= a.length) {
      return b[bStart + k - 1];
    }
    if(bStart >= b.length) {
      return a[aStart + k - 1];
    }
    if(k == 1) {
      return Math.min(a[aStart], b[bStart]);
    }
    int aMidIdx = Math.min(a.length - 1, aStart + k / 2 - 1);
    int bMidIdx = Math.min(b.length - 1, bStart + k / 2 - 1);
    if(a[aMidIdx] < b[bMidIdx]) {
      return kth(a, b, k - aMidIdx + aStart - 1, aMidIdx + 1, bStart);
    } else {
      return kth(a, b, k - bMidIdx + bStart - 1, aStart, bMidIdx + 1);
    }
  }
}
```