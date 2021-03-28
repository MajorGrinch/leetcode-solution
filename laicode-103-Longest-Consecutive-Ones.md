# Laicode 103. Longest Consecutive 1s

给定一个只有01的数组，求出最长全1子数组的长度。

## DP Approach

简单的动态规划，`ones`数组存放子问题的答案，`ones[i]`的意义是以`i`结尾的最长全1子数组长度。

那么我们可以看出来，如果`array[i]`为0的话，那么`ones[i]`肯定就是0。如果`array[i]`不为0的话，那么`ones[i] = ones[i - 1] + 1`。`ones[0]`就取决于`array[0]`，然后我们从第二个下标开始遍历一直到最后，每轮循环更新`ones[i]`然后记录最长的全1数组长度。

Time complexity: O(N)

Space complexity: O(N)

```java
public class Solution {
  public int longest(int[] array) {
    if (array == null || array.length == 0) {
      return 0;
    }
    int n = array.length;
    int[] ones = new int[n];
    ones[0] = array[0] == 1 ? 1 : 0;
    int longestOnes = ones[0];
    for (int i = 1; i < n; i++) {
      ones[i] = array[i] == 1 ? ones[i - 1] + 1 : 0;
      longestOnes = Math.max(longestOnes, ones[i]);
    }
    return longestOnes;
  }
}
```

## Greedy Approach

贪心思想来解决，`st`记录当前全1子数组的起始下标，初始化为`-1`。`i`从头开始向后遍历，如果`array[i]`是0，那么`st`必然不可能是`i`，至少得从后一个开始。如果`array[i]`是1，那么先看看`st`是否还是`-1`，如果是的话，说明这是数组里第一个1，我们要对`st`进行赋值。然后再去计算最长的全1子数组长度。

Time complexity: O(N)

Space complexity: O(1)

```java
public class Solution {
  public int longest(int[] array) {
    if (array == null || array.length == 0) {
      return 0;
    }
    int n = array.length;
    int st = -1;
    int res = 0;
    for (int i = 0; i < n; i++) {
      if (array[i] == 0) {
        st = i + 1;
      } else {
        if (st == -1) {
          st = i;
        }
        res = Math.max(res, i - st + 1);
      }
    }
    return res;
  }
}
```