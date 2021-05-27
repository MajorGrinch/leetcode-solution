# Laicode 202. Kth Smallest In Two Sorted Arrays

这题采用而二分的思想。把`a`数组和`b`数组的各自前`k/2`个比较，达到每次删除k/2的效果。A[k/2 - 1]和B[k/2 - 1]谁小就删谁的前k/2，因为第k小肯定不在那个范围里，该范围里面都是比第k小还小的数。我们只要在剩下的的范围里寻找`k - k/2`小就好。

Time complexity: O(logk)

Space complexity: O(logk) because of logk level call stack.

```java
public class Solution {
  public int kth(int[] a, int[] b, int k) {
    return kth(a, b, k, 0, 0);
  }

  private int kth(int[] a, int[] b, int k, int aStart, int bStart) {
    if (aStart >= a.length) {
      return b[bStart + k - 1];
    }
    if (bStart >= b.length) {
      return a[aStart + k - 1];
    }
    if (k == 1) {
      return Math.min(a[aStart], b[bStart]);
    }
    int aMidIndex = aStart + k / 2 - 1;
    int bMidIndex = bStart + k / 2 - 1;
    int aMidVal = aMidIndex >= a.length ? Integer.MAX_VALUE : a[aMidIndex];
    int bMidVal = bMidIndex >= b.length ? Integer.MAX_VALUE : b[bMidIndex];
    if (aMidVal < bMidVal) {
      return kth(a, b, k - k / 2, aMidIndex + 1, bStart);
    } else {
      return kth(a, b, k - k / 2, aStart, bMidIndex + 1);
    }
  }
}
```