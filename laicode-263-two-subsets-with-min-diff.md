# Laicode 263. Two Subsets With Min Difference

把一个数字对半分，要求两半的差值最小，返回这个最小的差值。

这题其实还是DFS的思路，沿途记录全局数组之和total，当前的和curr以及对应的size，全局答案数组res。

向下搜的时候有两种情况
+ 当前index加入curr，那么对应size + 1
+ 当前index不加入已经搜索的范围，对应size不变

一只搜到size是数组长度一半的时候就可以停了，对比当前curr和total - curr差值的绝对值和全局答案数组res，更新res。

Time complexity: O(2^n)

Space complexity: O(n)

```java
public class Solution {
  public int minDifference(int[] array) {
    int total = 0;
    for(int n : array) {
      total += n;
    }
    int[] res = {Integer.MAX_VALUE};
    dfs(array, 0, total, 0, 0, res);
    return res[0];
  }

  private void dfs(int[] array, int index, int total, int curr, int size, int[] res) {
    if(size == array.length / 2) {
      res[0] = Math.min(res[0], Math.abs(total - curr - curr));
    }

    if(index == array.length) {
      return;
    }
    dfs(array, index + 1, total, curr + array[index], size + 1, res);
    dfs(array, index + 1, total, curr, size, res);
  }
}
```