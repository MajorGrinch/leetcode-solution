# 34. Find First and Last Position of Element in Sorted Array

## CH

这题主要是target可能会出现很多次，先是左右边界检查确定target确实在数组范围内。

然后去找第一个出现的地方，如果找到了再找最后一次出现的地方。

这题主要是找最后一次出现的地方，last occurrence。我们二分搜索想要维护的loop invariant是：last occurrence出现在[l, r]区间里。

当nums[mid] <= target时，last occurrence肯定在[mid, r]里。否则，last occurrence肯定出现在[l, mid - 1]里。这样我们可以维持循环不变量。

但是这里有个细节要注意，mid = l + (r-l)/2 这个运算决定了，`mid`是有可能等于 `l` 的。如果`mid`等于`l`并且`nums[mid] <= target`，这题就会进入死循环。因为我们维护循环不变量会使得l = mid。为了防止死循环，我们必须让`mid`永远不可能等于`l`。怎么办？循环条件改为 r - l > 1。这样进入循环的话，算出来的`mid`永远不可能等于`l`。

这么做的代价就是最后需要额外检查一下l和r，并且返回。

```java
class Solution {
    private final int[] noAns = {-1, -1};
    public int[] searchRange(int[] nums, int target) {
        if(nums == null || nums.length == 0){
            return noAns;
        }
        if(nums[0] > target || nums[nums.length - 1] < target){
            return noAns;
        }

        int[] ans = {firstOccurrence(nums, target), -1};
        if(ans[0] == -1){
            return ans;
        }else{
            ans[1] = lastOccurrence(nums, target);
            return ans;
        }

    }

    private int firstOccurrence(int[] nums, int target){
        int l = 0, r = nums.length - 1;
        while(l < r){
            int mid = l + (r-l >> 1);
            if(nums[mid] < target){
                l = mid + 1;
            }else{
                r = mid;
            }
        }
        return nums[l] == target ? l : -1;
    }

    /**
     * Call this only if firstOccurrence doesn't return -1.
     */
    private int lastOccurrence(int[] nums, int target){
        int l = 0, r = nums.length - 1;
        while(r - l > 1){
            int mid = l + (r-l >> 1);
            if(nums[mid] <= target){
                l = mid;
            }else{
                r = mid - 1;
            }
        }
        return nums[r] == target ? r : l;
    }
}
```