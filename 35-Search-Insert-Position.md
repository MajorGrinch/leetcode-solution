# 35. Search Insert Position

## CH

这题说白了就是找第一个大于等于target的数。

先来左右边界检查，接着是二分搜索第一个GE的数。

循环不变量是：我们要找的数始终落在[l, r]这个闭区间内。

当nums[mid] < target时，这个数肯定在[mid + 1, r]里。当nums[mid] >= target时，这个数肯定在[l, mid]里。通过这样来维持循环不变量。

最后结束的时候，l == r，说明第一个GE的数就是这个。

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        if(nums == null || nums.length == 0){
            return -1;
        }
        if(nums[0] >= target){
            return 0;
        }else if(nums[nums.length - 1] < target){
            return nums.length;
        }else{
            return firstGE(nums, target);
        }
    }

    /**
     * Find the first one that is greater than or equal to target.
     */
    private int firstGE(int[] nums, int target){
        int l = 0, r = nums.length - 1;
        while(l < r){
            int mid = l + (r-l >> 1);
            if(nums[mid] < target){
                l = mid + 1;
            }else{
                r = mid;
            }
        }
        return l;
    }
}
```