# Laicode 69. Missing Number II

这题和[268. Missing Number](268-Missing-Number.md)比较像，不过给定的输入是升序的。因此，我们可以利用其他方法。

## Iterate approach

按序遍历，位置`i`放的数应当是`i+1`，对不上那就是缺少`i+1`。如果都对上了，那么就缺少`len + 1`。

Time complexity: O(n)

Space complexity: O(1)

```java
public class Solution {
  public int missing(int[] array) {
    for (int i = 0; i < array.length; i++) {
      if (array[i] != i + 1) return i + 1;
    }
    return array.length + 1;
  }
}
```

## Binary Search

这题还可以用二分法的思想。根据我们上一个方法的思路，会出现第一个错位的元素即`array[i] != i + 1`。我们通过二分法找到首个错位元素，其实和first bad version很像。

Time complexity: O(logn)

Space complexity: O(1)

```java
public class Solution {
  public int missing(int[] array) {
    if (array == null || array.length == 0) {
      return 1;
    }
    int n = array.length;
    if (array[0] != 1) {
      return 1;
    }
    if (array[n - 1] == n) {
      return n + 1;
    }
    int l = 0, r = n - 1;
    while (l < r) {
      int mid = l + (r - l >> 1);
      if (array[mid] != mid + 1) {
        r = mid;
      } else {
        l = mid + 1;
      }
    }
    return l + 1;
  }
}
```