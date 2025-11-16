# Laicode 116. Array Deduplication II

参照[laicode 80](laicode-80-Remove-Adjacent-Repeated-Char-II.md)。

```java
public class Solution {
  public int[] dedup(int[] array) {
    if(array == null || array.length <= 2) {
      return array;
    }
    int slow = 0;
    int fast = 0;
    while(fast < array.length) {
      int begin = fast;
      while(fast < array.length && array[fast] == array[begin]) {
        fast++;
      }
      for(int i = 0; i < 2 && begin + i < fast; i++) {
        array[slow++] = array[begin + i];
      }
    }
    int[] res = new int[slow];
    for(int i = 0; i < slow; i++) {
      res[i] = array[i];
    }
    return res;
  }
}
```