# Laicode 26. Kth Smallest Number In Sorted Matrix

给定一个横向纵向都是升序的二维数组，找到第K大的数。参照[Laicode 194](laicode-194-Kth-Closest-Point-To-000.md)的思路。

用一个`Coord`类来记录坐标和对应的值，重写`compareTo`方法来实现对比。用BFS的思想从`matrix[0][0]`开始访问，每次都把相邻两个方向没访问过的元素放入堆中。

重复K-1次，那么此时堆顶的就是第K小的元素。

Time complexity: O(klogk)

Space complexity: O(m * n), m is matrix rows and n is matrix cols


```java
public class Solution {
  public int kthSmallest(int[][] matrix, int k) {
    PriorityQueue<Coord> minHeap = new PriorityQueue<>();
    boolean[][] vis = new boolean[matrix.length][matrix[0].length];

    minHeap.offer(new Coord(0, 0, matrix[0][0]));
    vis[0][0] = true;

    for(int i = 0; i < k - 1; i++) {
      Coord curr = minHeap.poll();
      int row = curr.row;
      int col = curr.col;
      if(row + 1 < matrix.length && !vis[row + 1][col]) {
        minHeap.offer(new Coord(row + 1, col, matrix[row + 1][col]));
        vis[row + 1][col] = true;
      }
      if(col + 1 < matrix[0].length && !vis[row][col + 1]) {
        minHeap.offer(new Coord(row, col + 1, matrix[row][col + 1]));
        vis[row][col + 1] = true;
      }
    }
    return minHeap.peek().val;
  }
}

class Coord implements Comparable<Coord> {
  int row;
  int col;
  int val;

  public Coord(int row, int col, int val) {
    this.row = row;
    this.col = col;
    this.val = val;
  }

  @Override
  public int compareTo(Coord c2) {
    return Integer.compare(this.val, c2.val);
  }
}
```