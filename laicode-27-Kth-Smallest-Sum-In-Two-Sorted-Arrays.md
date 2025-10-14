# Laicode 27. Kth Smallest Sum In Two Sorted Arrays

给定两个有序数组，分别从两个数组里各取一个元素相加，找到第K小的和。

这题可以用BFS配合小顶堆来解决。直观的看，最小的和肯定是首元素的和对应的下标组合也就是`(0, 0)`，那么第二小肯定是`(0, 1)`或者`(1, 0)`。至于第三小的话，那就不一定了。假如第二小是`(0, 1)`，那么`(1, 0)`、`(1, 1)`和`(2, 0)`都有可能是第三小。

所以我们可以得到一个推论，假如当前组合`(x, y)`是第m小的和，那么`(x + 1, y)`和`(x, y + 1)`也**可能**是第m+1小，至于到底是不是那得让小顶堆说了算。因此，我们用一个小顶堆来记录组合，按照元素和作为比较条件。每从堆顶拿出一个组合，把它对应的两个相邻的组合放入堆里。重复k-1次，这样堆顶就是和第k小的组合了。

需要注意的是，我们需要一个`vis`数组来记录已经入堆的组合。为什么？仔细想一想。实在不明白，可以参考[Laicode 194](laicode-194-Kth-Closest-Point-To-000.md)里面的解释。

Time complexity: O(klogk)

Space complexity: O(m * n)

```java
public class Solution {
  public int kthSum(int[] A, int[] B, int k) {
    // sanity check if necessary
    int m = A.length, n = B.length;
    boolean[][] vis = new boolean[m][n];
    PriorityQueue<Comb> minHeap =
        new PriorityQueue<>(
            new Comparator<Comb>() {
              @Override
              public int compare(Comb c1, Comb c2) {
                return Integer.compare(A[c1.ai] + B[c1.bi], A[c2.ai] + B[c2.bi]);
              }
            });

    vis[0][0] = true;
    minHeap.offer(new Comb(0, 0));
    for (int i = 0; i < k - 1; i++) {
      Comb curr = minHeap.poll();
      int ai = curr.ai, bi = curr.bi;
      if (ai + 1 < m && !vis[ai + 1][bi]) {
        minHeap.offer(new Comb(ai + 1, bi));
        vis[ai + 1][bi] = true;
      }
      if (bi + 1 < n && !vis[ai][bi + 1]) {
        minHeap.offer(new Comb(ai, bi + 1));
        vis[ai][bi + 1] = true;
      }
    }
    Comb resComb = minHeap.peek();
    return A[resComb.ai] + B[resComb.bi];
  }

  class Comb {
    int ai;
    int bi;

    Comb(int ai, int bi) {
      this.ai = ai;
      this.bi = bi;
    }
  }
}
```