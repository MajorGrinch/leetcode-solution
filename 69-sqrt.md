# 69. Sqrt

用二分法找根号。

假定根号结果在[l, r]区间内，去逼近。要注意的是，根据根号结果向下取整的要求，当`mid^2 < x`，我们需要`l = mid`。所以循环条件需要用专属的`while (r - l) > 1`。

```java
class Solution {
    public int mySqrt(int x) {
        if (x < 2)
            return x;

        int l = 1;
        int r = x / 2;
        while (r - l > 1) {
            int mid = l + (r - l) / 2;
            long sq = (long) mid * mid;
            if (sq == x) {
                return (int) mid;
            } else if (sq < x) {
                l = mid;
            } else {
                // x < sq
                r = mid - 1;
            }
        }
        if ((long) r * r <= x) {
            return r;
        } else {
            return l;
        }
    }
}
```