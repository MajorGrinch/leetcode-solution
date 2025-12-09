# 154. Find Minimum in Rotated Sorted Array II

和[153. Find Minimum in Rotated Sorted Array](153-Find-Min-Rotated-Sorted-Array.md)差不多，但是这题给定的数组会有重复元素。

我们还是照例二分搜索，选定当前区间的最右端点的元素作为支点比较，并分类讨论：
+ 当`nums[mid] < nums[r]`时候，`[mid, r]`就是升序区间。这说明`mid`在断崖右侧，最小值肯定在`[l, mid]`里。
+ 当`nums[mid] > nums[r]`时候，说明`mid`在断崖左侧，那么最小值肯定在`(mid, r]`里。
+ 当上述两个条件都不满足的时候，即`nums[mid] == nums[r]`。我们没有办法判断`mid`到底在断崖前还是后，但是我们可以判断 `nums[r]` 可以不作为最小值考虑。为什么？如果 `nums[r]`是最小值，那么 `nums[mid]` 同样也是最小值，我们后面会继续逼近的。

Time complexity: O(logn)

Space complexity: O(1)

```java
class Solution {
  public int findMin(int[] nums) {
    int l = 0, r = nums.length - 1;
    while (l < r) {
      int mid = l + (r - l >> 1);
      if(nums[mid] < nums[r]){
        r = mid;
      }else if(nums[mid] > nums[r]){
        l = mid + 1;
      }else{
        r--;
      }
    }
    return nums[l];
  }
}
```