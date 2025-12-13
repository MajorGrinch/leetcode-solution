# 378. Kth Smallest Element in a Sorted Matrix

题目给定一个横竖都有序的矩阵，要求找到第K小的数。

## BFS + Min Heap

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

## Binary Search

这题我们可以针对二维数组进行二分搜索。怎么搜呢？根据它横竖都是有序的性质，我们知道 `matrix[0][0]` 是最小的，`matrix[n - 1][n - 1]` 是最大的，他们会有一个平均值 mid。我们遍历一下数组，统计有多少个数小于等于这个 mid，
+ 如果正好有 K 个数小于等于 mid，那么这 K 个里面最大的数不就是我们要找的答案了吗？！
+ 如果不足 K 个数小于等于 mid，那我们可以把第 K 大的数的范围锁定到大于 mid。
+ 如果大于 K 个数小于等于 mid，那么第 K 大的数的范围锁定是小于等于 mid。

按照这个方法，再继续二分，一直到小于等于 mid 的数正好为 K 为止。那么此时，小于等于 mid 最大的数就是我们要找的答案。

还有一个问题，在这个二维数组里找到小于等于某个特定值 target，要怎么找？需要遍历全部元素吗？不用，我们从第 0 行最后一个元素开始，如果该元素小于等于 target，那么这一行所有元素都满足，并且我们挪到下一行。如果该元素大于 target，那么这一列就都不满足，我们挪到前一列。这样子，我们最后就可以统计出来有多少个元素小于等于 target。不过我们沿途还有记录两个东西，一个是小于等于 target 的数字里最大的数，另一个是大于 target 的数字里最小的数。为什么要记录这俩？因为二分的时候，我们需要他们来锁定答案范围。

因为这题保证答案肯定是有的，所以我们二分的条件就是 `while(l < r)`。如果循环是以 `l == r` 而结束的话，那么 `l`就是第 K 大的数。

Time complexity: O(N * log(max value - min value))

Space complexity: O(1)

```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        int l = matrix[0][0];
        int r = matrix[n - 1][n - 1];
        while (l < r) {
            int mid = l + (r - l) / 2;
            int[] smallLargePair = new int[] { matrix[0][0], matrix[n - 1][n - 1] };
            int count = countLE(matrix, mid, smallLargePair);
            if (count == k) {
                return smallLargePair[0];
            } else if (count > k) {
                r = smallLargePair[0];
            } else {
                l = smallLargePair[1];
            }
        }
        return l;
    }

    private int countLE(int[][] matrix, int target, int[] smallLargePair) {
        int count = 0;
        int n = matrix.length;
        int r = 0;
        int c = n - 1;
        while (r < n && c >= 0) {
            if (matrix[r][c] <= target) {
                count += c + 1;
                smallLargePair[0] = Math.max(smallLargePair[0], matrix[r][c]);
                r++;
            } else {
                smallLargePair[1] = Math.min(smallLargePair[1], matrix[r][c]);
                c--;
            }
        }
        return count;
    }
}
```