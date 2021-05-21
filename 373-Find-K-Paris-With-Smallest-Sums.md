# 373. Find K Pairs with Smallest Sums

和[laicode 27. Kth Smallest Sum In Two Sorted Arrays](laicode-27-Kth-Smallest-Sum-In-Two-Sorted-Arrays.md)一样，只不过这次我们K个都记录下来。

然后两种方法来搞小顶堆，要么自定义Comparator，要么Coord可以实现Comparable接口。不过放在leetcode里面跑，好像后者快一点，不知道回头研究一下。

Time complexity: O(klogk)

Space complexity: O(k)

```java
class Solution {
  public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
    int m = nums1.length, n = nums2.length;
    boolean[][] vis = new boolean[m][n];
    PriorityQueue<Coord> minHeap =
        new PriorityQueue<>(
            new Comparator<Coord>() {
              @Override
              public int compare(Coord c1, Coord c2) {
                return Integer.compare(nums1[c1.i1] + nums2[c1.i2], nums1[c2.i1] + nums2[c2.i2]);
              }
            });

    vis[0][0] = true;
    minHeap.offer(new Coord(0, 0));
    List<List<Integer>> res = new ArrayList<>();
    for (int i = 0; i < k && !minHeap.isEmpty(); i++) {
      Coord curr = minHeap.poll();
      int i1 = curr.i1, i2 = curr.i2;
      res.add(Arrays.asList(nums1[i1], nums2[i2]));
      if (i1 + 1 < m && !vis[i1 + 1][i2]) {
        minHeap.offer(new Coord(i1 + 1, i2));
        vis[i1 + 1][i2] = true;
      }
      if (i2 + 1 < n && !vis[i1][i2 + 1]) {
        minHeap.offer(new Coord(i1, i2 + 1));
        vis[i1][i2 + 1] = true;
      }
    }
    return res;
  }

  class Coord {
    int i1;
    int i2;

    Coord(int i1, int i2) {
      this.i1 = i1;
      this.i2 = i2;
    }
  }
}
```

```java
class Solution {
  public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
    int m = nums1.length, n = nums2.length;
    boolean[][] vis = new boolean[m][n];
    PriorityQueue<Tuple> minHeap = new PriorityQueue<>();

    vis[0][0] = true;
    minHeap.offer(new Tuple(0, 0, nums1[0] + nums2[0]));
    List<List<Integer>> res = new ArrayList<>();
    for (int i = 0; i < k && !minHeap.isEmpty(); i++) {
      Tuple curr = minHeap.poll();
      int i1 = curr.i1, i2 = curr.i2;
      res.add(Arrays.asList(nums1[i1], nums2[i2]));
      if (i1 + 1 < m && !vis[i1 + 1][i2]) {
        minHeap.offer(new Tuple(i1 + 1, i2, nums1[i1 + 1] + nums2[i2]));
        vis[i1 + 1][i2] = true;
      }
      if (i2 + 1 < n && !vis[i1][i2 + 1]) {
        minHeap.offer(new Tuple(i1, i2 + 1, nums1[i1] + nums2[i2 + 1]));
        vis[i1][i2 + 1] = true;
      }
    }
    return res;
  }

  class Tuple implements Comparable<Tuple> {
    int i1;
    int i2;
    int sum;

    Tuple(int i1, int i2, int sum) {
      this.i1 = i1;
      this.i2 = i2;
      this.sum = sum;
    }

    @Override
    public int compareTo(Tuple another) {
      return Integer.compare(this.sum, another.sum);
    }
  }
}
```