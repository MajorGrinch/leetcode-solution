# 33. Search in Rotated Sorted Array

给定一个无重复元素的升序数组，把他循环右移一部分然后在里面寻找`target`。找得到就返回下标，否则返回`-1`。

Clarification:
+ 数组无重复元素

这题我们用二分搜索。虽然数组被循环右移之后不是严格的升序了，但是二分搜索依然有用。这个数组循环右移了若干位，我们可以知道这个数组中间会出现一个断崖，断崖左边是最大的数而右边是最小的数。二分搜索的时候`[l, mid)`和`(mid, r]`这两个区间至少有一个区间是完全落在断崖的一侧的，也就是至少有一个区间是完全升序的。这不就好办了？`target`要是落在那个升序区间里，那就往那个升序区间缩。如果不在，那就往另一个区间缩。

我们选定当前搜索区间的最右侧元素`nums[r]`作为`pivot`，二分搜索时候分类讨论：
+ `nums[mid] == target`，找到了，直接返回`mid`
+ `nums[r] < nums[mid]`，说明`mid`在断崖左侧，也就是`[l, mid]`是完全升序的。如果`target`落在`[l, mid)`里的话，就往`[l, mid)`缩。否则就往`(mid, r]`缩。
+ `nums[r] > nums[mid]`，说明`mid`在断崖右侧，也就是说`[mid, r]`是完全升序的。如果`target`落在`(mid, r]`里的话，就往`(mid, r]`缩。否则就往`[l, mid)`缩。

Time complexity: O(logn)

Space complexity: O(1)

```java
class Solution {
    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        int l = 0;
        int r = nums.length - 1;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] > nums[r]) {
                // mid before cliff, [l, mid] is acending
                if (nums[l] <= target && target < nums[mid]) {
                    r = mid - 1;
                } else {
                    l = mid + 1;
                }
            } else {
                // mid after cliff, [mid, r] is acending
                if (nums[mid] < target && target <= nums[r]) {
                    l = mid + 1;
                } else {
                    r = mid - 1;
                }
            }
        }
        return -1;
    }
}
```