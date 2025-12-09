# 162. Find Peak Element

这题有几个 assumption:
+ 任意相邻的数字都不相等
+ 数组不为空
+ 都是 int 范围内的数字

说来大家可能不信，这题就是最简单的二分法。`nums[mid] < nums[mid + 1]` 可以得出结论 mid 右边肯定有一个顶点。`nums[mid] > nums[mid + 1]` 可以得出结论 mid 自己 或者 mid 左边是顶点。`nums[mid] == nums[mid + 1]`，根据题目的 assumption 不可能。

就这样，逼近到最后就是顶点。

Time complexity: O(logN)

Space complexity: O(1)

```java
class Solution {
    public int findPeakElement(int[] nums) {
        int l = 0;
        int r = nums.length - 1;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (nums[mid] < nums[mid + 1]) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        return l;
    }
}
```