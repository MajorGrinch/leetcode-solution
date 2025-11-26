# 407. Trapping Rain Water II

给定一个高度数组，求出它能盛放多少水。

这题相当于是2维版的[42. Trapping Rain Water](42-Trapping-Rain-Water.md)，但是解法大不相同。这题我们需要用到小顶堆来把所有的边界上的点压入，排序标准就是高度。接着，我们开始用BFS来把边界向内收缩，过程中我们就可以计算可以存放多少水。

当堆不为空的时候做以下几件事：
+ 从队列中取出最矮的点，遍历它的未访问过的相邻点。
+ 如果相邻点比它矮，那么这个相邻点就可以盛他们之间高度差体积的水。如果相邻点不比它矮，那就盛不了水。
+ 把相邻点压入队列。如果相邻点比当前点高的话，那就取相邻点的高度。如果矮的话，就取当前点的高度，因为当前点是短板。

重复上述步骤直至队列为空，也就是我们在不断地收缩边界，这时候整个矩阵能存放的水体积就显而易见了。


```java
class Solution {
  public int trapRainWater(int[][] heightMap) {
    if (heightMap.length == 0 || heightMap[0].length == 0) {
      return 0;
    }
    PriorityQueue<Cell> boarder = new PriorityQueue<>();
    int m = heightMap.length, n = heightMap[0].length;
    boolean[][] vis = new boolean[m][n];
    preprocessBorder(heightMap, vis, boarder);
    int vol = 0;

    int[] di = {0, 0, 1, -1}, dj = {1, -1, 0, 0};
    while (!boarder.isEmpty()) {
      Cell lowest = boarder.poll();
      for (int k = 0; k < 4; k++) {
        int ni = lowest.i + di[k];
        int nj = lowest.j + dj[k];
        if (0 <= ni && ni < m && 0 <= nj && nj < n && !vis[ni][nj]) {
          vol += Math.max(heightMap[ni][nj], lowest.h) - heightMap[ni][nj];
          vis[ni][nj] = true;
          boarder.offer(new Cell(ni, nj, Math.max(heightMap[ni][nj], lowest.h)));
        }
      }
    }
    return vol;
  }

  private void preprocessBorder(int[][] heightMap, boolean[][] vis, PriorityQueue<Cell> pq) {
    int m = heightMap.length, n = heightMap[0].length;
    for (int i = 0; i < m; i++) {
      pq.offer(new Cell(i, 0, heightMap[i][0]));
      pq.offer(new Cell(i, n - 1, heightMap[i][n - 1]));
      vis[i][0] = true;
      vis[i][n - 1] = true;
    }
    for (int j = 1; j < n - 1; j++) {
      pq.offer(new Cell(0, j, heightMap[0][j]));
      pq.offer(new Cell(m - 1, j, heightMap[m - 1][j]));
      vis[0][j] = true;
      vis[m - 1][j] = true;
    }
  }

  class Cell implements Comparable<Cell> {
    int i;
    int j;
    int h;

    Cell(int i, int j, int h) {
      this.i = i;
      this.j = j;
      this.h = h;
    }

    @Override
    public int compareTo(Cell another) {
      return Integer.compare(this.h, another.h);
    }
  }
}
```

2025 code:

```java
class Solution {
    public int trapRainWater(int[][] heightMap) {
        if (heightMap.length == 0 || heightMap[0].length == 0) {
            return 0;
        }
        PriorityQueue<Cell> boarder = new PriorityQueue<>();
        int m = heightMap.length;
        int n = heightMap[0].length;
        boolean[][] vis = new boolean[m][n];
        preprocessBorder(heightMap, vis, boarder);
        int vol = 0;

        int[] di = { 0, 0, 1, -1 };
        int[] dj = { 1, -1, 0, 0 };
        while (!boarder.isEmpty()) {
            Cell lowest = boarder.poll();
            for (int k = 0; k < di.length; k++) {
                int ni = lowest.i + di[k];
                int nj = lowest.j + dj[k];
                if (0 <= ni && ni < m && 0 <= nj && nj < n && !vis[ni][nj]) {
                    vol += Math.max(heightMap[ni][nj], lowest.h) - heightMap[ni][nj];
                    vis[ni][nj] = true;
                    boarder.offer(new Cell(ni, nj, Math.max(heightMap[ni][nj], lowest.h)));
                }
            }
        }
        return vol;
    }

    private void preprocessBorder(int[][] heightMap, boolean[][] vis, PriorityQueue<Cell> border) {
        int m = heightMap.length;
        int n = heightMap[0].length;
        for (int i = 0; i < m; i++) {
            border.offer(new Cell(i, 0, heightMap[i][0]));
            border.offer(new Cell(i, n - 1, heightMap[i][n - 1]));
            vis[i][0] = true;
            vis[i][n - 1] = true;
        }
        for (int j = 1; j < n - 1; j++) {
            border.offer(new Cell(0, j, heightMap[0][j]));
            border.offer(new Cell(m - 1, j, heightMap[m - 1][j]));
            vis[0][j] = true;
            vis[m - 1][j] = true;
        }
    }

    class Cell implements Comparable<Cell> {
        int i;
        int j;
        int h;

        Cell(int i, int j, int h) {
            this.i = i;
            this.j = j;
            this.h = h;
        }

        @Override
        public int compareTo(Cell another) {
            return Integer.compare(this.h, another.h);
        }
    }
}
```