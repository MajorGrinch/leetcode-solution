# Laicode 188. 4 Sum

给定一个数组，判断里面是否存在和为`target`的四元组 (i1, j1, i2, j2)。

我们用一个单独的类`Pair`来表示一个双元组，我们遍历一次数组得到所有可能的two sum对应的双元组(i1, j1)。注意，这里(i1, j1)都要尽可能的小。怎么实现？看代码。

再用双重循环来遍历另外两个元素 `(i2, j2)` 的情况，只要`target - array[i2] - array[j2]`出现在Map里，并且双元组的j1小于当前的i2，那么这就是个存在的和为`target`的四元组。第二次循环的(i2, j2)可以从数组末尾开始，可以尽可能的保证(i1 < j1 < i2 < j2)，从而尽快找到四元组。

Time complexity: O(N^2)

Space complexity: O(N^2)

```java
public class Solution {
  public boolean exist(int[] array, int target) {
    Map<Integer, Pair> sumPairMap = new HashMap<>();
    for(int j = 1; j < array.length; j++) {
      for(int i = 0; i < j; i++) {
        int sum = array[i] + array[j];
        if(!sumPairMap.containsKey(sum)) {
          sumPairMap.put(sum, new Pair(i, j));
        }     
      }
    }
    for(int i = array.length - 2; i >= 0; i--) {
      for(int j = i + 1; j < array.length; j++) {
        int sum = array[i] + array[j];
        if(sumPairMap.containsKey(target - sum)) {
          Pair p = sumPairMap.get(target - sum);
          if(p.j < i) {
            return true;
          }
        }
      }
    }
    return false;
  }

  class Pair {
    int i;
    int j;

    Pair(int i, int j) {
      this.i = i;
      this.j = j;
    }
  }
}
```