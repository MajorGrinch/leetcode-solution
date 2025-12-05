# 704. Binary Search

简单的二分法搜索题。

我们先假定target出现在nums数组里，那么循环不变量(loop invariant)是：target出现在[l, r]这个闭区间里。

当mid等于target时，就直接返回。当nums[mid] < target时，target肯定出现在[mid + 1, r]区间里。当nums[mid] > target时，target肯定出现在[l, mid - 1]区间里。这样，我们始终维持着loop invariant。

循环终止条件是 l >= r，那么很有可能最后l > r 或者 l == r。但是，这并不要紧，因为我们的循环维持的循环不变量是 target 出现在[l, r]这个闭区间里。如果l > r的话，说明target根本没出现。如果l == r的话，就看看nums[l]到底是不是target就好了。

Time complexity: O(logN)

Space complexity: O(1)

```java
class Solution {
    public int search(int[] nums, int target) {
        if(nums == null || nums.length == 0){
            return -1;
        }
        int l = 0, r = nums.length - 1;
        while(l < r){
            int mid = l + (r-l>>1);
            if(nums[mid] == target){
                return mid;
            }else if(nums[mid] < target){
                l = mid + 1;
            }else{
                r = mid - 1;
            }
        }
        return nums[l] == target ? l : -1;
    }
}
```

2025 update：把循环终止条件变成 l > r，loop invariant 维持不变。如果 target 出现在 `[l, r]` 里，那中途就应该被找到然后返回。如果循环结束了都没找到，那意思很明确，就是没有，那直接返回 -1。

2025 code:

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
            } else if (nums[mid] < target) {
                l = mid + 1;
            } else {
                r = mid - 1;
            }
        }
        return -1;
    }
}
```