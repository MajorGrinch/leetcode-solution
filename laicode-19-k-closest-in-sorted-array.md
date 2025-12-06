# Laicode 19. K Closest In Sorted Array

给定一个数组，和一个 target，找出数组里离 target最近的 K 个数。按照离 target 的远近升序输出。

Assumption:
+ k >= 0
+ k <= array.length
+ 两个数离 target 一样距离，小的那个数在先

这题很简单， 在数组里找到第一个大于等于 target 的坐标 firstGEIdx，参照[leetcode 35](35-Search-Insert-Position.md)找。以这个坐标为起点开始左右搜寻，l 初始化为 firstGEIdx - 1，r初始化为 firstGEIdx，谁小挪谁。如果 l 或者 r 碰到边界的话，就挪另一个就好。

Time compelxity: O(logn + k)

Space complexity: O(k)

```java
public class Solution {
  public int[] kClosest(int[] array, int target, int k) {
    if(array.length == 0 || k == 0) {
      return new int[0];
    }
    int[] res = new int[k];
    int n = array.length;
    if(array[n - 1] < target) {
      for(int i = 0; i < k; i++) {
        res[i] = array[n - 1 - i];
      }
      return res;
    }
    int firstGEIdx = firstGE(array, target);
    int r = firstGEIdx;
    int l = r - 1;
    int i = 0;
    while(i < k) {
      if(l < 0) {
        res[i++] = array[r++];
      } else if (r > n - 1) {
        res[i++] = array[l--];
      } else if (Math.abs(array[r] - target) < Math.abs(array[l] - target)) {
        res[i++] = array[r++];
      } else {
        res[i++] = array[l--];
      }
    }
    return res;

  }

  /**
  Guarantee at least one element >= target
  */
  private int firstGE(int[] array, int target) {
    int l = 0;
    int r = array.length - 1;
    while(l < r) {
      int mid = l + (r - l) / 2;
      if(array[mid] >= target) {
        r = mid;
      } else {
        l = mid + 1;
      }
    }
    return l;
  }
}
```