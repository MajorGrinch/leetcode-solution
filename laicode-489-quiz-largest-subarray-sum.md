# Laicode 489. Quiz: Largest Subarray Sum

参照[laicode 97](laicode-97-largest-subarray-sum.md)，只不过更新答案的时候顺带更新左右 index。

```java
public class Solution {
  public int[] largestSum(int[] array) {
    int currSum = array[0];
    int currLeft = 0;
    int currRight = 0;
    int resSum = currSum;
    int resLeft = 0;
    int resRight = 0;

    for(int i = 1; i < array.length; i++) {
      if(currSum > 0) {
        currSum += array[i];
      } else {
        currSum = array[i];
        currLeft = i;
      }
      currRight = i;
      if(currSum > resSum) {
        resSum = currSum;
        resLeft = currLeft;
        resRight = currRight;
      }
    }

    return new int[]{resSum, resLeft, resRight};
  }
}
```