# Laicode 22. Search In Shifted Sorted Array II

和[81. Search in Rotated Sorted Array II](81-Search-In-Rotated-Sorted-Array-II.md)差不多，但是这里要求返回首个出现的下标。这里就很不一样了，前者是找到就行，这题是不仅要找到还要找到第一个。

所以我们在`array[mid] == target`的时候，把`r`提到`mid`来缩放范围。但是这样还是不够，有些情况比如`3,1,1,1,1,3`，我们会误认为3在`1,1,3`里。所以我们上来首先检查，`array[l] == target`，如果相等的话就直接把`r`提到`l`来结束搜索。因为正如我在上一题说的，我们搜过的过程其实不能保证`target`一定在`[l, r]`里，但是我们可以保证`target`如果有的话只可能在`[l, r]`里。所以，如果`array[l] == target`的话，它就是第一个首次出现。

处理了`array[l] == target`和`array[mid] == target`这两种情况后，其他情况就和上一题一样。

Time complexity: O(logn)

Space complexity: O(1)

```java
public class Solution {
  public int search(int[] array, int target) {
    if (array == null || array.length == 0) {
      return -1;
    }
    int l = 0, r = array.length - 1;
    while (l < r) {
      int mid = l + (r - l >> 1);
      if (array[l] == target) {
        r = l;
      } else if (array[mid] == target) {
        r = mid;
      } else if (array[l] < array[mid] || array[mid] > array[r]) {
        // [l, mid) ascending
        if (array[l] <= target && target < array[mid]) {
          r = mid - 1;
        } else {
          l = mid + 1;
        }
      } else if (array[l] > array[mid] || array[mid] < array[r]) {
        if (array[mid] < target && target <= array[r]) {
          l = mid + 1;
        } else {
          r = mid - 1;
        }
      } else {
        r--;
      }
    }
    return array[l] == target ? l : -1;
  }
}

```