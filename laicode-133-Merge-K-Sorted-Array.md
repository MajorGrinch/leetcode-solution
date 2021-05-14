# Laicode 133. Merge K Sorted Array

讲真，这题和[23. Merge k Sorted Lists](23-Merge-K-Sorted-Lists.md)差不多，一个思路。只是这题是二维数组，所以实现小顶堆的`Comparator`的时候略有区别，其他要做的事情是一样的。

时空复杂度也是一样的。

```java
public class Solution {
  public int[] merge(int[][] arrayOfArrays) {
    // sanity check
    int k = arrayOfArrays.length;
    if (k == 0) return new int[] {};
    PriorityQueue<Coord> minHeap =
        new PriorityQueue<>(
            new Comparator<Coord>() {
              @Override
              public int compare(Coord c1, Coord c2) {
                return Integer.compare(arrayOfArrays[c1.i][c1.j], arrayOfArrays[c2.i][c2.j]);
              }
            });
    int N = 0;
    for (int i = 0; i < k; i++) {
      N += arrayOfArrays[i].length;
      if (arrayOfArrays[i].length > 0) minHeap.offer(new Coord(i, 0));
    }
    int[] merged = new int[N];
    int i = 0;
    while (!minHeap.isEmpty()) {
      Coord curr = minHeap.poll();
      merged[i++] = arrayOfArrays[curr.i][curr.j];
      if (curr.j + 1 < arrayOfArrays[curr.i].length) minHeap.offer(new Coord(curr.i, curr.j + 1));
    }
    return merged;
  }

  class Coord {
    int i;
    int j;

    Coord(int i, int j) {
      this.i = i;
      this.j = j;
    }
  }
}
```