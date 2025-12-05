# Laicode 16. Last Occurrence

参照[leetcode 34](34-Find-First-and-Last-Position.md)保证有 occurrence 的情况，但是这题不保证有 occurrence，上来就要直接找最后一个位置。

那么还是按照那题的逻辑找，最后分别检查 `r` 和 `l` 是否等于 target，都不是就返回 `-1`。

```java
public class Solution {
  public int lastOccur(int[] array, int target) {
    if(array == null || array.length == 0) {
      return -1;
    }
    int l = 0;
    int r = array.length - 1;
    while(r - l > 1) {
      int mid = l + (r - l) / 2;
      if(array[mid] <= target) {
        l = mid;
      } else {
        r = mid - 1;
      }
    }
    if(array[r] == target) {
      return r;
    } else if(array[l] == target) {
      return l;
    } else {
      return -1;
    }
  }
}
```