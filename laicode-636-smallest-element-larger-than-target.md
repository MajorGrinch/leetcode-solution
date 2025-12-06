# Laicode 636. Smallest Element Larger than Target

在升序数组里，找到第一个比target大的数。参照[leetcode 744](744-Find-Smallest-Letter-Greater-Than-Target.md)。

assumption:
+ 有重复元素
+ 找不到返回-1

```java
public class Solution {
  public int smallestElementLargerThanTarget(int[] array, int target) {
    if(array == null || array.length == 0) {
      return -1;
    }
    int n = array.length;
    if(array[n - 1] <= target) {
      return -1;
    }
    int l = 0;
    int r = n - 1;
    while(l < r) {
      int mid = l + (r - l) / 2;
      if(array[mid] <= target) {
        l = mid + 1;
      } else {
        r = mid;
      }
    }
    return l;
  }
}
```