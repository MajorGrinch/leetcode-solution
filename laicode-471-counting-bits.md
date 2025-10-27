# Laicode 471. Counting Bits

## Count Individually Approach

给定一个数字，从0到这个数里每个数的二进制有多少个1。算出来，放入个数组并且返回。

这题用bit operation，每个数字用32次循环可以计算出有多少个1，算好了放入结果数组。

Time complexity: O(n)

Space complexity: O(n)，因为结果数组需要n。

```java
public class Solution {
  public int[] countBits(int num) {
    int[] res = new int[num + 1];
    for(int n = 0; n <= num; n++) {
      int count = 0;
      for(int i = 0; i <= 31; i++) {
        if(((n >> i) & 1) == 1) {
          count++;
        }
      }
      res[n] = count;
    }
    return res;
  }
}
```

## Dynamic Programming Approach

这题稍加思考就知道满足动态规划的条件。比如5的二进制是`0101`，那么5的二进制有多少个1取决于什么呢？取决于最低位是否是1，外加5/2有几个1。

唉？是不是有所启发？后面结果永远取决于前面计算的结果，一眼DP，秒了。

Time complexity: O(n)

Space complexity: O(n)

```java
public class Solution {
  public int[] countBits(int num) {
    int[] res = new int[num + 1];
    res[0] = 0;
    for(int n = 1; n <= num; n++) {
      res[n] = res[n >> 1] + (n & 1);
    }
    return res;
  }
}
```