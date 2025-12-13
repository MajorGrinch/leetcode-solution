# 852. Peak Index in a Mountain Array

最最基本的二分法，不多说了。

Time complexity: O(logN)

Space complexity: O(1)

```java
class Solution {
    public int peakIndexInMountainArray(int[] arr) {
        int l = 0;
        int r = arr.length - 1;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (arr[mid] < arr[mid + 1]) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        return l;
    }
}
```