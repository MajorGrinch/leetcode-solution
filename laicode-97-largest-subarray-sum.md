# Laicode 97. Largest SubArray Sum

## DP Approach

参照[leetcode 53](53-Maximum-Subarray.md)。

```java
public class Solution {
  public int largestSum(int[] array) {
    int len = array.length;
    int[] sum = new int[len];
    sum[0] = array[0];
    int res = sum[0];
    for(int i = 1; i < len; i++) {
      if(sum[i - 1] > 0) {
        sum[i] = sum[i - 1] + array[i];
      } else {
        sum[i] = array[i];
      }
      res = Math.max(res, sum[i]);
    }
    return res;
  }
}
```


## Greedy Approach

还是参照[leetcode 53](53-Maximum-Subarray.md)。

```java
public class Solution {
  public int largestSum(int[] array) {
    int currSum = array[0];
    int res = currSum;
    for(int i = 1; i < array.length; i++) {
      if(currSum > 0) {
        currSum += array[i];
      } else {
        currSum = array[i];
      }
      res = Math.max(res, currSum);
    }
    return res;
  }
}
```