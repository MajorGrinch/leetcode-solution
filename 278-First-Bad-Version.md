# 278. First Bad Version

## CH

说白了是找first occurrence。

循环不变量(loop invariant)是first bad version出现在[l, r]区间里。

如果mid is not bad verison，那么first bad version肯定出现在[mid + 1, r]。如果mid is bad version，那么first bad version肯定出现在[l, mid]。这样就可以维持loop invariant。

最后循环终止，l == r，就是first bad version。

```java
/* The isBadVersion API is defined in the parent class VersionControl.
      boolean isBadVersion(int version); */

public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int l = 0, r = n;
        while(l < r){
            int mid = l + (r-l >> 1);
            if(isBadVersion(mid)){
                r = mid;
            }else{
                l = mid + 1;
            }
        }
        return l;
    }
}
```