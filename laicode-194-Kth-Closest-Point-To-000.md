# Laicode 194. Kth Closest Point To <0,0,0>

给定三个不含负数的升序数组，求出由他们各出一个数组成的三维坐标里离原点第K近的数。

和[laicode 27. Kth Smallest Sum In Two Sorted Arrays](laicode-27-Kth-Smallest-Sum-In-Two-Sorted-Arrays.md)差不多，只不过前者是求和，这题是求与原点之间的欧几里得距离。那么其实就是三维空间的BFS配合小顶堆，实现上没什么区别。

我们用一个`Tuple`类来记录三个坐标来自于三个数组的哪几个数，并且计算离原点的距离，并且重写compareTo方法来根据距离比较。

接着用一个小顶堆来，首先`(a[0], b[0], c[0])`肯定是离原点最近的。那么接下来呢？到底是`(a[1], b[0], c[0])`，`(a[0], b[1], c[0])`还是`(a[0], b[0], c[1])`是第二近？聪明的你肯定想到了，第二近必定在他们三个之间，所以他们三个全都入堆。

以此类推，我们每从小顶堆里面弹出一个`(a[ai], b[bi], c[ci])`，就把`(a[ai + 1], b[bi], c[ci])`，`(a[ai], b[bi + 1], c[ci])`和`(a[ai], b[bi], c[ci + 1])`都放入堆。重复k-1次这样的操作，这样最后堆顶就是第K近的点。

这就够了吗？不，我们还需要一个`vis`数组来记录已经入堆的坐标点防止重复。因为我们每次都是三个方向分别推进，难免会有重复。比如`(a[1], b[0], c[1])`可以是`a[1], b[0], c[0 + 1]`，也可以是`(a[0 + 1], b[0], c[1])`，所以必须要记录已经入堆的坐标防止重复。

Time complexity: O(klogk)

Space complexity: O(k^3)

```java
public class Solution {
  public List<Integer> closest(int[] a, int[] b, int[] c, int k) {
    // sanity check if necessary
    boolean[][][] vis = new boolean[k][k][k];
    PriorityQueue<Tuple> minHeap = new PriorityQueue<>();

    vis[0][0][0] = true;
    minHeap.offer(new Tuple(0, 0, 0, a[0], b[0], c[0]));
    for (int i = 0; i < k - 1; i++) {
      Tuple curr = minHeap.poll();
      int ai = curr.ai, bi = curr.bi, ci = curr.ci;
      if (ai + 1 < a.length && !vis[ai + 1][bi][ci]) {
        vis[ai + 1][bi][ci] = true;
        minHeap.offer(new Tuple(ai + 1, bi, ci, a[ai + 1], b[bi], c[ci]));
      }
      if (bi + 1 < b.length && !vis[ai][bi + 1][ci]) {
        vis[ai][bi + 1][ci] = true;
        minHeap.offer(new Tuple(ai, bi + 1, ci, a[ai], b[bi + 1], c[ci]));
      }
      if (ci + 1 < c.length && !vis[ai][bi][ci + 1]) {
        vis[ai][bi][ci + 1] = true;
        minHeap.offer(new Tuple(ai, bi, ci + 1, a[ai], b[bi], c[ci + 1]));
      }
    }
    Tuple res = minHeap.peek();
    return Arrays.asList(a[res.ai], b[res.bi], c[res.ci]);
  }

  class Tuple implements Comparable<Tuple> {
    int ai, bi, ci;
    int aVal, bVal, cVal;
    Long dis;

    Tuple(int ai, int bi, int ci, int aVal, int bVal, int cVal) {
      this.ai = ai;
      this.bi = bi;
      this.ci = ci;
      this.aVal = aVal;
      this.bVal = bVal;
      this.cVal = cVal;
      this.dis = (long) aVal * aVal + bVal * bVal + cVal * cVal;
    }

    @Override
    public int compareTo(Tuple another) {
      return Long.compare(this.dis, another.dis);
    }
  }
}
```