# Laicode 86. Longest Ascending SubArray

参照[leetcode 674](674-LCIS.md)。

```java
public class Solution {
  public int longest(int[] array) {
    int n = array.length;
    if(n <= 1) {
      return n;
    }
    int[] increasing = new int[n];
    increasing[0] = 1;
    int res = 1;
    for(int i = 1; i < n; i++) {
      if(array[i] > array[i - 1]) {
        increasing[i] = increasing[i - 1] + 1;
      } else {
        increasing[i] = 1;
      }
      res = Math.max(res, increasing[i]);
    }
    return res;
  }
}
```