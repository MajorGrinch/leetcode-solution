# Laicode 206. Majority Number I

参照[leetcode 169](169-Majority-Element.md)。

```java
public class Solution {
  public int majority(int[] array) {
    int candidate = array[0];
    int count = 1;
    for(int i = 1; i < array.length; i++) {
      if(array[i] == candidate) {
        count++;
      } else if(count == 0) {
        candidate = array[i];
        count++;
      } else {
        count--;
      }
    }
    return candidate;
  }
}
```