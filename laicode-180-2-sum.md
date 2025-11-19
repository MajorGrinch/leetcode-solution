# Laicode 180. 2 Sum

参照[leetcode 1](1-Two-Sum.md)。不过这题不用给出坐标，所以我们就用 Set 来存放已经发现的数字就好。

```java
public class Solution {
  public boolean existSum(int[] array, int target) {
    Set<Integer> numSet = new HashSet<>();
    for(int n: array) {
      if(numSet.contains(target - n)) {
        return true;
      }
      numSet.add(n);
    }
    return false;
  }
}
```