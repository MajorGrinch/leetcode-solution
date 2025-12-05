# 35. Search Insert Position

这题说白了就是找第一个大于等于target的数。

先来左右边界检查，接着是二分搜索第一个GE的数。循环不变量是：我们要找的数始终落在[l, r]这个闭区间内。当nums[mid] < target时，这个数肯定在[mid + 1, r]里。当nums[mid] >= target时，这个数肯定在[l, mid]里。通过这样来维持循环不变量。最后结束的时候，l == r，说明第一个GE的数就是这个。

注意，以上方法需要一个前提，即该数组存在大于等于 target 的数。所以我们一开始先用一个 if 排除该数组不存在大于等于 target 的数这一情况，然后再处理。

Time complexity: O(logN)

Space complexity: O(1)

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        if (target > nums[nums.length - 1]) {
            return nums.length;
        }
        return firstGE(nums, target);
    }

    private int firstGE(int[] nums, int target) {
        int l = 0;
        int r = nums.length - 1;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (nums[mid] >= target) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```