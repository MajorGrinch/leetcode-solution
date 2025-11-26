# Laicode 199. Max Water Trapper I

参照[leetcode 42](42-Trapping-Rain-Water.md)。

```java
public class Solution {
  public int maxTrapped(int[] array) {
    if(array.length <= 2) {
      return 0;
    }
    int len = array.length;
    int[] leftPeak = new int[len];
    leftPeak[0] = array[0];
    for(int i = 1; i < len; i++) {
      leftPeak[i] = Math.max(leftPeak[i - 1], array[i]);
    }
    int[] rightPeak = new int[len];
    rightPeak[len - 1] = array[len - 1];
    for(int i = len - 2; i >= 0; i--) {
      rightPeak[i] = Math.max(rightPeak[i + 1], array[i]);
    }
    int vol = 0;
    for(int i = 1; i < len - 1; i++) {
      vol += Math.min(leftPeak[i], rightPeak[i]) - array[i];
    }
    return vol;
  }
}
```