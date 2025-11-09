# Laicode 88. Array Hopper I

参照[leetcode 88](55-Jump-Game.md)。

```java
public class Solution {
  public boolean canJump(int[] array) {
    int n = array.length;
    boolean[] reachable = new boolean[n];
    reachable[0] = true;
    for(int i = 0; i < n - 1; i++) {
      if(!reachable[i]) {
        continue;
      }
      for(int j = 1; j <= array[i]; j++) {
        if(i + j < n) {
          reachable[i + j] = true;
        }
      }
    }
    return reachable[n - 1];
  }
}
```