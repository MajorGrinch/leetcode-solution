# 153. Find Minimum in Rotated Sorted Array

给定被循环右移过后的一个升序数组，找到最小的数。

Clarification:
+ 无重复元素

这题还是二分搜索，我们知道最小元素肯定是断崖右边那个元素，我们选取搜索区间的右端点作为支点比较，并进行分类讨论：
+ 如果`nums[mid] > nums[r]`说明`mid`在断崖左边，那么最小值一定在`(mid, r]`里。
+ 如果`nums[mid] < nums[r]`，说明`mid`在断崖右边但是我们并不知道是不是紧挨着断崖，最小值在`[l, mid]`里。

那么我们为什么取搜索区间的右端点？因为如果存在断崖的话，最小值肯定出现在右端点的左边或者本身。如果没有断崖的话，最小值也在右端点的左边。所以我们通过对比`nums[mid]`和右端点，我们可以沿着`nums[r]`提供给我们的信息来毕竟`nums[r]`左边的那个最小值。

Time complexity: O(logn)

Space complexity: O(1)

```java
class Solution {
  public int findMin(int[] nums) {
    int l = 0, r = nums.length - 1;
    while (l < r) {
      int mid = l + (r - l >> 1);
      if (nums[mid] > nums[r]) {
        l = mid + 1;
      } else {
        r = mid;
      }
    }
    return nums[l];
  }
}
```

2025 update:

和[33. Search in Rotated Sorted Array](33-Search-in-Rotated-Sorted-Array.md)类似，采用一般的二分搜索，找顶点。找到顶点之后，右边那个就是最小的元素。不过我们首先要保证这题有悬崖，如果没有悬崖的话，就没必要费劲找了。


```java
class Solution {
    public int findMin(int[] nums) {
        int n = nums.length;
        if (n == 1) {
            return nums[0];
        }
        if (nums[0] < nums[n - 1]) {
            // no cliff
            return nums[0];
        }
        int l = 0;
        int r = nums.length - 1;
        int pivot = nums[0];
        while (r - l > 1) {
            int mid = l + (r - l) / 2;
            if (pivot < nums[mid]) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return nums[l] <= nums[r] ? nums[r + 1] : nums[r];
    }
}
```