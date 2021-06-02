# 81. Search in Rotated Sorted Array II

和[33. Search in Rotated Sorted Array](33-Search-in-Rotated-Sorted-Array.md)差不多，但是会有重复元素，所以我们之前来判定哪个区间是升序的方法就不可行了。

那么这题我们依然有方法可以判定`[l, mid)`和`（mid, r]`谁是升序区间。分类讨论：
+ `nums[l] < nums[mid]`或者`nums[mid] > nums[r]`，那么`[l, mid)`肯定是升序区间。前者很直观，后者则是证明`mid`在断崖之前。
+ `nums[l] > nums[mid]`或者`nums[mid] < nums[r]`，那么`(mid, r]`肯定是升序区间。后者很直观，前者则是证明`mid`在断崖之后。

如果上述两种情况都不满足，那么我们可以推导出`nums[l] == nums[mid] == nums[r]`。这时候`mid`所在位置就不好说了，所以我们将`r`前移一位，继续搜索。为什么这么做？虽然我们不能确定`target`在`[l, r]`，但是我们可以确定`target`肯定不在`[, r]`以外的地方。既然`nums[l] == nums[mid] == nums[r] != target`，那么就把`r--`来强制缩小范围，以方便我们后续搜索。

Time complexity: O(logn)

Space complexity: O(1)

```java
class Solution {
  public boolean search(int[] nums, int target) {
    if (nums.length == 0) {
      return false;
    }
    int l = 0, r = nums.length - 1;
    while (l <= r) {
      int mid = l + (r - l >> 1);
      if (nums[mid] == target) {
        return true;
      } else if (nums[l] < nums[mid] || nums[mid] > nums[r]) {
        // [l, mid] is in ascending order
        if (nums[l] <= target && target < nums[mid]) {
          r = mid - 1;
        } else {
          l = mid + 1;
        }
      } else if (nums[l] > nums[mid] || nums[mid] < nums[r]) {
        // [mid, r] is in ascending order
        if (nums[mid] < target && target <= nums[r]) {
          l = mid + 1;
        } else {
          r = mid - 1;
        }
      } else {
        // nums[l] == nums[mid] == nums[r]
        r--;
      }
    }
    return false;
  }
}
```