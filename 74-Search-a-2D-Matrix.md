# 74. Search a 2D Matrix

还是二分搜索，分两步走。

第一步先找到哪一行有可能存在target，这里我们预设的Loop invariant是target一定出现在[l, r]这些行里。如果`matrix[mid][0] <= target && target <= matrix[mid][n-1]` 的话，那么target就在这一行，直接进去再二分找。如果matrix[mid][0] > target，那么target只有可能在[l, mid - 1]这些行里。如果matrix[mid][n - 1] > target，那么target只可能在[mid + 1, r]这些行里。这样就可以维持loop invariant。

第二步在行内找target，loop invariant是target一定出现在[l, r]这个区间里。通过操作维护循环不变量，最后返回。

但是这里我写了两个版本的行内查找，主要区别是`while(l < r)`和`while(l <= r)`。关于这俩循环的区别，看[二分搜索专题](Binary-Search.md)。

Time complexity: O(log(m) + log(n))

Space complexity: O(1)

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int l = 0;
        int r = matrix.length - 1;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            int[] row = matrix[mid];
            if (inside(row, target)) {
                return binSearch2(row, target);
            } else if (target < row[0]) {
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return false;
    }

    private boolean inside(int[] nums, int target) {
        return nums[0] <= target && target <= nums[nums.length - 1];
    }

    private boolean binSearch(int[] nums, int target) {
        int l = 0;
        int r = nums.length - 1;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            int val = nums[mid];
            if (val == target) {
                return true;
            } else if (val < target) {
                l = mid + 1;
            } else {
                // val > target
                r = mid - 1;
            }
        }
        return false;
    }

    private boolean binSearch2(int[] nums, int target) {
        int l = 0;
        int r = nums.length - 1;
        while (l < r) {
            int mid = l + (r - l) / 2;
            int val = nums[mid];
            if (val == target) {
                return true;
            } else if (val < target) {
                l = mid + 1;
            } else {
                // val > target
                r = mid - 1;
            }
        }
        return nums[l] == target;
    }
}
```

2025 code:

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        int m = matrix.length;
        int n = matrix[0].length;
        int l = 0;
        int r = m - 1;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            if (matrix[mid][0] <= target && target <= matrix[mid][n - 1]) {
                return binSearch(matrix[mid], target) != -1;
            } else if (target < matrix[mid][0]) {
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return false;
    }

    private int binSearch(int[] nums, int target) {
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