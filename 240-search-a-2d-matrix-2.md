# 240. Search a 2D Matrix II

给定一个二维数组，横向纵向都是升序的，要求返回该数组是否含有 target。

那么我们从第 0 行的最后一个元素开始，
+ 遇到相同的元素，直接返回 true
+ 当前元素比 target 小，那么这一行 [0, c] 所有元素都排除，我们就直接去下一行。
+ 当前元素比 target 大，那么这一列 [r, m - 1]所有元素都可以排除，我们直接去之前的那一列找。

如果最后行或者列碰壁了，也没找到，那就是没有。

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        int m = matrix.length;
        int n = matrix[0].length;
        int r = 0;
        int c = n - 1;
        while (r < m && c >= 0) {
            if (matrix[r][c] == target) {
                return true;
            } else if (matrix[r][c] < target) {
                r++;
            } else {
                c--;
            }
        }
        return false;
    }
}
```