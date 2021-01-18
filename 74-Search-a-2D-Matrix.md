# 74. Search a 2D Matrix

## CH

还是二分搜索，分两步走。

第一步先找到哪一行有可能存在target，这里我们预设的Loop invariant是target一定出现在[l, r]这些行里。如果`matrix[mid][0] <= target && target <= matrix[mid][n-1]` 的话，那么target只可能在这一行，直接break。如果matrix[mid][0] > target，那么target只有可能在[l, mid - 1]这些行里。如果matrix[mid][n - 1] > target，那么target只可能在[mid + 1, r]这些行里。这样就可以维持loop invariant。

第二步在行内找target，loop invariant是target一定出现在[l, r]这个区间里。通过操作维护循环不变量，最后返回。

但是这里我写了两个版本的行内查找，主要区别是`while(l < r)`和`while(l <= r)`。关于这俩循环的区别，看[二分搜索专题](Binary-Search.md)。

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if(matrix == null || matrix.length == 0 || matrix[0].length == 0){
            return false;
        }
        int m = matrix.length, n = matrix[0].length;
        int l = 0, r = m - 1;
        int targetRow = -1;
        while(l <= r){
            int mid = l + (r-l >> 1);
            if(matrix[mid][0] <= target && target <= matrix[mid][n - 1]){
                targetRow = mid;
                break;
            }else if(matrix[mid][0] > target){
                r = mid - 1;
            }else{
                l = mid + 1;
            }
        }
        if(targetRow == -1){
            return false;
        }else {
            return binSearch(matrix[targetRow], target);
        }
    }

    private boolean binSearch(int[] nums, int target){
        int l = 0, r = nums.length - 1;
        while(l < r){
            int mid = l + (r-l>>1);
            if(nums[mid] == target){
                return true;
            }else if(nums[mid] < target){
                l = mid + 1;
            }else{
                r = mid - 1;
            }
        }
        return nums[l] == target;
    }

    private boolean binSearch2(int[] nums, int target){
        int l = 0, r = nums.length - 1;
        while(l <= r){
            int mid = l + (r-l>>1);
            if(nums[mid] == target){
                return true;
            }else if(nums[mid] < target){
                l = mid + 1;
            }else{
                r = mid - 1;
            }
        }
        return false;
    }
}
```