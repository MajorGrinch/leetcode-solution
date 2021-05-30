# Laicode 683. Count Ascending Subsequence

## Dynamic Programming Approach

用`numIS`来记录以每个下标结尾的升序序列个数，那么`numIS[i] = SUM(numIS[j]) + 1, j < i && a[j] < a[i]`，`+1`是因为`numIS[i]`本身也是个升序序列。最后返回`numIS`之和就好。

Time complexity: O(n^2)

Space complexity: O(n)

```java
public class Solution {
  public int numIncreasingSubsequences(int[] a) {
    if (a.length <= 1) {
      return a.length;
    }
    int n = a.length;
    int[] numIS = new int[n];
    numIS[0] = 1;
    for (int i = 1; i < n; i++) {
      int numISBf = 0;
      for (int j = 0; j < i; j++) {
        if (a[j] < a[i]) numISBf += numIS[j];
      }
      numIS[i] = numISBf + 1; // a[i] it self is also a ascending subsequence
    }
    int res = 0;
    for (int num : numIS) res += num;
    return res;
  }
}
```