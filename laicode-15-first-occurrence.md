# Laicode 15. First Occurrence

参照[leetcode 34](34-Find-First-and-Last-Position.md)。

```java
public class Solution {
  public int firstOccur(int[] array, int target) {
    if(array == null || array.length == 0) {
      return -1;
    }
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
    return array[l] == target ? l : -1;
  }
}
```