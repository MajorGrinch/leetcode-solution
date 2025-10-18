# 378. Kth Smallest Element in a Sorted Matrix

题目给定一个横竖都有序的矩阵，要求找到第K小的数。

这题典型的BFS搜索，配合堆的使用，这次我们用`Java`的`PriorityQueue`来替代手写堆。这里堆里存坐标，但是是根据坐标对应在矩阵里面的值进行排序，所以我们实现优先队列的时候需要自定义一个`Comparator`。此外，我们还需要一个同等大小的vis矩阵来记录当前坐标是否被访问过，以防止重复访问。

从(0, 0)开始BFS搜索，每次把堆顶的坐标(x, y)弹出来，将它未访问过的(x + 1, y)和(x, y + 1)邻居放入队列，并把他们标记为已访问。如此循环`k-1`次，那么此时堆顶的坐标就是这个矩阵里第`k`小的元素了。

Time complexity: O(klogk)，因为会重复k-1次，堆最多不到2k个元素，所以offer操作需要logk的复杂度。

Space complexity: O(k + n^2)，堆里会有不到2k个元素，额外的需要一个和矩阵同等大小的vis数组来记录访问情况。


2025 update:

这题多了个有意思的条件，要求空间复杂度必须小于O(n^2)。那么我们经典的BFS+MinHeap就需要做一点小修改，不可以用vis数组来记录已经访问过的点了。那怎么办呢？

我们可以把第一列的前K行元素都入堆，我可以保证第K小的元素肯定是在这K行里，然后循环K-1次。每次从堆顶弹出最小的坐标，然后把他下一列入堆。循环结束后，堆顶就是第K小的元素了。

Space complexity: O(k)

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

2025 code:

```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        PriorityQueue<Pair> minHeap = new PriorityQueue<>();
        int n = matrix.length;
        for (int i = 0; i < Math.min(n, k); i++) {
            minHeap.offer(new Pair(i, 0, matrix[i][0]));
        }
        for (int i = 0; i < k - 1; i++) {
            Pair curr = minHeap.poll();
            int r = curr.r;
            int c = curr.c;
            if (c + 1 < n) {
                minHeap.offer(new Pair(r, c + 1, matrix[r][c + 1]));
            }
        }
        return minHeap.poll().v;
    }
}

class Pair implements Comparable<Pair> {
    int r;
    int c;
    int v;

    public Pair(int r, int c, int v) {
        this.r = r;
        this.c = c;
        this.v = v;
    }

    @Override
    public int compareTo(Pair p2) {
        return Integer.compare(this.v, p2.v);
    }
}
```