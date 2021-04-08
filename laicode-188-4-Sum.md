# Laicode 188. 4 Sum

给定一个数组，判断里面是否存在和为`target`的四元组。

首先我们把数组进行排序，方便操作。我们用一个`TwoSumPair`来表示一个双元组他们的和对应某个值，我们遍历一次数组得到所有可能的two sum对应的双元组(i1, j1)的Map，每一个和对应的双元组要选取首次出现的地方。

再用双重循环来遍历另外两个元素的情况，只要`target - array[i2] - array[j2]`出现在Map里，并且双元组的j1小于当前的i2，那么这就是个存在的和为`target`的四元组。

Time complexity: O(N^2)

Space complexity: O(N^2)

```java
public class Solution {
  public boolean exist(int[] array, int target) {
    if (array.length <= 3) {
      return false;
    }
    Arrays.sort(array);
    Map<Integer, TwoSumPair> twoSumPairMap = new HashMap<>();
    for (int j = 1; j < array.length; j++) {
      for (int i = 0; i < j; i++) {
        int sum = array[i] + array[j];
        if (!twoSumPairMap.containsKey(sum)) {
          twoSumPairMap.put(sum, new TwoSumPair(i, j));
        }
      }
    }
    for (int j = 1; j < array.length; j++) {
      for (int i = j - 1; i >= 0; i--) {
        int remain = target - array[i] - array[j];
        TwoSumPair pair = twoSumPairMap.get(remain);
        if (pair != null && pair.j < i) {
          return true;
        }
      }
    }
    return false;
  }

  class TwoSumPair {
    int i;
    int j;

    TwoSumPair(int i, int j) {
      this.i = i;
      this.j = j;
    }
  }
}
```