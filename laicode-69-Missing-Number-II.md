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

这题还可以用二分法的思想。根据我们上一个方法的思路，会出现第一个错位的元素即`array[i] != i + 1`。我们通过二分法找到首个错位元素，其实和[first bad version](278-First-Bad-Version.md)很像。

用 first bad version 那样二分查找，只适用于跳过了中间某个数字的情况，即 `[1...N)` 某个元素被跳过了。为什么？因为如果 `[1...N]` 只是最后的 `N` 被省略了，那数组就不存在 first bad version，那怎么找？对吧？所以我们首先要排除这一情况之后，再进行二分搜索。最后 l 停的位置就是那个被跳过元素本应该在的位置，`l + 1` 就是那个元素的值。

Time complexity: O(logn)

Space complexity: O(1)

```java
public class Solution {
  public int missing(int[] array) {
    if(array.length == 0) {
      return 1;
    }
    if(array[array.length - 1] != array.length + 1) {
      return array.length + 1;
    }
    int l = 0;
    int r = array.length - 1;
    while(l < r) {
      int mid = l + (r - l) / 2;
      if(array[mid] == mid + 1) {
        l = mid + 1;
      } else {
        r = mid;
      }
    }
    return l + 1;
  }
}
```