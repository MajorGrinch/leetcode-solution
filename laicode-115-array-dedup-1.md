# Laicode 115. Array Deduplication I

参照[leetcode 26](26-Remove-Duplicates-From-Sorted-Array.md)。

```java
public class Solution {
  public int[] dedup(int[] array) {
    if(array == null || array.length <= 1) {
      return array;
    }
    int slow = 0;
    int fast = 0;
    while(fast < array.length) {
      int begin = fast;
      while(fast < array.length && array[fast] == array[begin]) {
        fast++;
      }
      array[slow++] = array[begin];
    }
    int [] res = new int[slow];
    for(int i = 0; i < slow; i++) {
      res[i] = array[i];
    }
    return res;
  }
}
```