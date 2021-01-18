# 74. Search a 2D Matrix

## CH

还是二分搜索，分两步走。

第一步先找到哪一行有可能存在target，这里我们预设的Loop invariant是target一定出现在[l, r]这些行里。如果`matrix[mid][0] <= target && target <= matrix[mid][n-1]` 的话，那么target只可能在这一行，直接break。如果matrix[mid][0] > target，那么target只有可能在[l, mid - 1]这些行里。如果matrix[mid][n - 1] > target，那么target只可能在[mid + 1, r]这些行里。这样就可以维持loop invariant。

这我为了避免最后还要再检查一遍，所以`while(l <= r)`。这样我使得`l == mid == r`这种情况成为可能，所以在循环结束之前，如果有一行能放得下target，那就必然会被捕捉到并break。

第二步在行内找target，loop invariant是target一定出现在[l, r]这个区间里。通过操作维护循环不变量，最后返回。

但是这里我写了两个版本的行内查找，主要区别是`while(l < r)`和`while(l <= r)`。第二种循环有可能遇到`l == mid == r`这么个情况，所以如果target真的在的话，必然会在某一轮循环里被找到直接返回true。如果这个循环能一直执行到`l > r`然后结束的话，那么说明target不在，我就有自信直接返回false。

当然了，这样写不太适合初学者理解，纯当炫技就好。我一般还是更倾向于`while(l < r)`这个，因为我们假定的循环不变量是要找的数在[l, r]之间，区间缩短到只有1个数时就看看那个数是否是target，缩短到0个数说明没有target。

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