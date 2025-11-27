# Laicode 19. K Closest In Sorted Array

给定一个数组，和一个 target，找出数组里离 target最近的 K 个数。按照离 target 的远近升序输出。

Assumption:
+ k >= 0
+ k <= array.length
+ 两个数离 target 一样距离，小的那个数在先

这题很简单， 在数组里找到第一个大于等于 target 的坐标 firstGEIdx，以这个坐标为起点开始左右搜寻。l 初始化为 firstGEIdx - 1，r初始化为 firstGEIdx，物理意义是 `(l, r)`区间是离 target 最近的 K 个数。那么很显然，结束条件是，`r - l - 1 == k`。

Time compelxity: O(logn + k)

Space complexity: O(k)

```java
public class Solution {
  public int[] kClosest(int[] array, int target, int k) {
    if(k == 0) {
      return new int[0];
    }
    int[] res = new int[k];
    if(target <= array[0]) {
      for(int i = 0; i < k; i++) {
        res[i] = array[i];
      }
      return res;
    }
    if(array[array.length - 1] <= target) {
      for(int i = 0; i < k; i++) {
        res[i] = array[array.length - 1 - i];
      }
      return res;
    }
    int firstGEIdx = firstGE(array, target);
    int l = firstGEIdx - 1;
    int r = firstGEIdx;
    int i = 0;
    while(r - l - 1 < k) {
      if(l < 0) {
        res[i++] = array[r++];
      } else if (r >= array.length) {
        res[i++] = array[l--];
      } else if(Math.abs(array[l] - target) > Math.abs(array[r] - target)) {
        res[i++] = array[r++];
      } else {
        res[i++] = array[l--];
      }
    }
    return res;
  }

  private int firstGE(int[] array, int target) {
    int l = 0;
    int r = array.length - 1;
    while(l < r) {
      int mid = l + (r - l) / 2;
      if(array[mid] < target) {
        l = mid + 1;
      } else {
        // a[mid] >= target
        r = mid;
      }
    }
    return r;
  }
}
```