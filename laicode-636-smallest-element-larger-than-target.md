# Laicode 636. Smallest Element Larger than Target

在升序数组里，找到第一个比target大的数。

assumption:
+ 有重复元素
+ 找不到返回-1

```java
public class Solution {
  public int smallestElementLargerThanTarget(int[] array, int target) {
    // Write your solution here
    if(array == null || array.length == 0) return -1;
    if(target < array[0]) return 0;
    if(array[array.length - 1] <= target) return -1;

    int l = 0;
    int r = array.length - 1;
    while(l < r) {
      int mid = l + (r-l) / 2;
      if(target < array[mid]) {
        r = mid;
      } else {
        // array[mid] <= target
        l = mid + 1;
      }
    }
    return l;
  }
}
```