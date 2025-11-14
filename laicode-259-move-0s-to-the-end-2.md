# Laicode 259. Move 0s To The End II

参照[leetcode 283](283-move-zeroes.md)。

```java
public class Solution {
  public int[] moveZero(int[] array) {
    int l = 0;
    for(int i = 0; i < array.length; i++) {
      if(array[i] != 0) {
        array[l++] = array[i];
      }
    }
    while(l < array.length) {
      array[l++] = 0;
    }
    return array;
  }
}
```