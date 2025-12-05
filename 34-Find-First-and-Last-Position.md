# 34. Find First and Last Position of Element in Sorted Array

这题主要是target可能会出现很多次，先是左右边界检查确定target确实在数组范围内，然后去找第一个出现的地方，如果找到了再找最后一次出现的地方。

找第一个出现的地方怎么找？`nums[mid] >= target` ==> `r = mid`，`nums[mid] < target` ==> `l = mid + 1`。循环终止条件是 `while(l < r)`。不用担心死循环，为什么？因为 `l = mid + 1` 搭配 `while(l < r)` 不会有死循环，切记不可搭配 `while(l <= r)`，否则会出现死循环。

找最后一次出现的地方，取决于first occur 是否找到。如果没有的话，那直接不用找。有的话，那可以保证有 occur。因此，我们二分搜索想要维护的loop invariant是：last occurrence出现在[l, r]区间里。`nums[mid] <= target` ==> `l = mid`，`nums[mid] > target` ==> `r = mid - 1`，这样维持循环不变量。但是这里有个细节要注意，`mid = l + (r - l) / 2` 这个运算决定了，`mid` 是有可能等于 `l` 的，那么 last occur 如果还用 `while(l < r)`就有可能进入死循环。为了防止死循环，我们必须让`mid`永远不可能等于`l`。怎么办？循环条件改为 r - l > 1。这样进入循环的话，算出来的`mid`永远不可能等于`l`。这么做的代价就是最后需要额外检查一下l和r，并且返回。

Time complexity: O(logN)

Space complexity: O(1)

小巧思：

在first occurrence 和 last occurrence的实现里，怎么写if else？这里有个简单的技巧。

以first occurrence为例，
+ `nums[mid] == target` -> `r = mid`
+ `nums[mid] < target` -> `l = mid + 1`
+ `target < nums[mid]` -> `r = mid - 1`

所以，第一个和第三个可以合并为
```java
if(target <= nums[i]) {
  r = mid;
} else {
  l = mid + 1;
}
```

同理，last occurrence时候，
+ `nums[mid] == target` -> `l = mid`
+ `nums[mid] < target` -> `l = mid + 1`
+ `target < nums[mid]` -> `r = mid - 1`

所以可以合并为
```java
if(nums[mid] <= target) {
  l = mid;
} else {
  r = mid - 1;
}
```
但是因为这个有`l = mid`，所以我们需要`while(r - l > 1)`来避免死循环。

AC code:

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

    private int lastOccurrence2(int[] nums, int target){
        int l = 0, r = nums.length - 1;
        while(l <= r){
            int mid = l + (r-l >> 1);
            if(nums[mid] == target){
                if(mid == nums.length - 1 || nums[mid + 1] > target){
                    return mid;
                }else{
                    l = mid + 1;
                }
            }else if(nums[mid] < target){
                l = mid + 1;
            }else{
                r = mid - 1;
            }
        }
        return -1;
    }
}
```

2025 code:

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return new int[] { -1, -1 };
        }
        if (target < nums[0] || nums[nums.length - 1] < target) {
            return new int[] { -1, -1 };
        }
        int firstIdx = firstOccur(nums, target);
        return firstIdx == -1 ? new int[] { -1, -1 } : new int[] { firstIdx, lastOccur(nums, target) };
    }

    private int firstOccur(int[] nums, int target) {
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
        return nums[l] == target ? l : -1;
    }

    /**
    Gurantee occurrence
     */
    private int lastOccur(int[] nums, int target) {
        int l = 0;
        int r = nums.length - 1;
        while (r - l > 1) {
            int mid = l + (r - l) / 2;
            if (nums[mid] <= target) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return nums[r] == target ? r : l;
    }
}
```