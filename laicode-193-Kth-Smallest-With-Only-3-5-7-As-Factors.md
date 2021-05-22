# Laicode 193. Kth Smallest With Only 3, 5, 7 As Factors

找到第K小的由3、5和7作为因数的数。

这题和[laicode 27. Kth Smallest Sum In Two Sorted Arrays](laicode-27-Kth-Smallest-Sum-In-Two-Sorted-Arrays.md)和[378. Kth Smallest Element in a Sorted Matrix](378-Kth-Smallest-Element-Sorted-Matrix.md)也都差不多思想，BFS + Min Heap。

无非就是控制3、5和7作为因数的个数，那么其实就是对一个三维的空间进行BFS。

Time complexity: O(klogk)

Space complexity: O(k^3)

```java
public class Solution {
  public long kth(int k) {
    boolean[][][] vis = new boolean[k + 1][k + 1][k + 1];
    PriorityQueue<Tuple> minHeap = new PriorityQueue<>();

    vis[1][1][1] = true;
    minHeap.offer(new Tuple(1, 1, 1, 3 * 5 * 7));
    for (int i = 1; i < k; i++) {
      Tuple curr = minHeap.poll();
      int x = curr.x, y = curr.y, z = curr.z;
      if (!vis[x + 1][y][z]) {
        vis[x + 1][y][z] = true;
        minHeap.offer(new Tuple(x + 1, y, z, curr.val * 3));
      }
      if (!vis[x][y + 1][z]) {
        vis[x][y + 1][z] = true;
        minHeap.offer(new Tuple(x, y + 1, z, curr.val * 5));
      }
      if (!vis[x][y][z + 1]) {
        vis[x][y][z + 1] = true;
        minHeap.offer(new Tuple(x, y, z + 1, curr.val * 7));
      }
    }
    return minHeap.peek().val;
  }

  class Tuple implements Comparable<Tuple> {
    int x;
    int y;
    int z;
    long val;

    Tuple(int x, int y, int z, long val) {
      this.x = x;
      this.y = y;
      this.z = z;
      this.val = val;
    }

    @Override
    public int compareTo(Tuple another) {
      return Long.compare(this.val, another.val);
    }
  }
}
```