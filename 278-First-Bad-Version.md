# 278. First Bad Version

## CH

说白了是找first occurrence。

循环不变量(loop invariant)是first bad version出现在[l, r]区间里。

如果mid is not bad verison，那么first bad version肯定出现在[mid + 1, r]。如果mid is bad version，那么first bad version肯定出现在[l, mid]。这样就可以维持loop invariant。

最后循环终止，l == r，就是first bad version。

这题不用担心死循环，因为mid为好，l = mid + 1

Time complexity: O(logN)

Space complexity: O(1)

```java
/* The isBadVersion API is defined in the parent class VersionControl.
      boolean isBadVersion(int version); */

public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int l = 0;
        int r = n;
        while (r - l > 1) {
            int mid = l + (r - l) / 2;
            if (isBadVersion(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return isBadVersion(l) ? l : r;
    }
}
```