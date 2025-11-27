# Laicode 202. Kth Smallest In Two Sorted Arrays

这题采用而二分的思想。把`a`数组和`b`数组的各自前`k/2`个比较，达到每次删除k/2的效果。A[k/2 - 1]和B[k/2 - 1]谁小就删谁的前k/2，因为第k小肯定不在那个范围里，该范围里面都是比第k小还小的数。我们只要在剩下的的范围里寻找`k - k/2`小就好。

有一点需要注意，`aStart + k / 2 -1`越界的时候，我们需要用 Integer.MAX_VALUE 来去掉 b 数组的 `[bStart, bStart + k / 2 -1]` 这 `k/2`个元素，因为第 k 大肯定不在这里。想想看为什么第 k 大肯定不在这里？反之，`bStart + k / 2 - 1`越界的时候，也是一个道理。

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

2025新写法，当`aStart + k / 2 - 1`越界的时候，我们直接取数组末尾的元素参与比较。同理，b 也是如此。但是有一个需要修改的地方就是，此刻不再是对于剩下的数组找`k - k/2`小的元素了，而是要看具体的 `[aStart, aMidIndex]` 有多少个元素。k 要减去这些元素的个数，作为新的 k 传下去。

```java
public class Solution {
  public int kth(int[] a, int[] b, int k) {
    return kth(a, b, k, 0, 0);
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
    int aMidIndex = Math.min(k / 2 + aStart - 1, a.length - 1);
    int bMidIndex = Math.min(k / 2 + bStart - 1, b.length - 1);
    int aMidVal = a[aMidIndex];
    int bMidVal = b[bMidIndex];
    if(aMidVal < bMidVal) {
      return kth(a, b, k - aMidIndex + aStart - 1, aMidIndex + 1, bStart);
    } else {
      return kth(a, b, k - bMidIndex + bStart - 1, aStart, bMidIndex + 1);
    }
  }
}

```