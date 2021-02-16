# 378. Kth Smallest Element in a Sorted Matrix

题目给定一个横竖都有序的矩阵，要求找到第K小的数。

这题典型的BFS搜索，配合堆的使用，这次我们用`Java`的`PriorityQueue`来替代手写堆。这里堆里存坐标，但是是根据坐标对应在矩阵里面的值进行排序，所以我们实现优先队列的时候需要自定义一个`Comparator`。此外，我们还需要一个同等大小的vis矩阵来记录当前坐标是否被访问过，以防止重复访问。

从(0, 0)开始BFS搜索，每次把堆顶的坐标(x, y)弹出来，将它未访问过的(x + 1, y)和(x, y + 1)邻居放入队列，并把他们标记为已访问。如此循环`k-1`次，那么此时堆顶的坐标就是这个矩阵里第`k`小的元素了。

Time complexity: O(klogk)，因为会重复k-1次，堆最多不到2k个元素，所以offer操作需要logk的复杂度。

Space complexity: O(k + n^2)，堆里会有不到2k个元素，额外的需要一个和矩阵同等大小的vis数组来记录访问情况。

```java
class Solution {
  public int kthSmallest(int[][] matrix, int k) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
      return -1;
    }
    PriorityQueue<int[]> pq =
        new PriorityQueue<>(
            k,
            new Comparator<int[]>() {
              public int compare(int[] c1, int[] c2) {
                int v1 = matrix[c1[0]][c1[1]];
                int v2 = matrix[c2[0]][c2[1]];
                return Integer.compare(v1, v2);
              }
            });
    int n = matrix.length;
    boolean[][] vis = new boolean[n][n];
    vis[0][0] = true;
    pq.offer(new int[] {0, 0});
    for (int i = 0; i < k - 1; i++) {
      int[] curr = pq.poll();
      int x = curr[0], y = curr[1];
      if (x + 1 < n && !vis[x + 1][y]) {
        pq.offer(new int[] {x + 1, y});
        vis[x + 1][y] = true;
      }
      if (y + 1 < n && !vis[x][y + 1]) {
        pq.offer(new int[] {x, y + 1});
        vis[x][y + 1] = true;
      }
    }
    int[] coor = pq.peek();
    return matrix[coor[0]][coor[1]];
  }
}
```