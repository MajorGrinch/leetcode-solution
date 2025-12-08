# 702. Search in a Sorted Array of Unknown Size

这题主要是array长度未知，所以我们要先确定二分搜索的左右边界。

先确定左右边界为0和1，不断地对对范围进行double来确定target是否在[l, r]之中。

敲定了target所在的范围之后，对这个范围进行二分搜索。

二分搜索的循环不变量是，target始终在[l, r]这个区间里。如果能找得到，那么一定会在循环结束之前找到并返回。如果循环顺利结束，l > r，说明没找到。

```java
class Solution {
    public int search(ArrayReader reader, int target) {
        int l = 0;
        int r = 1;
        while (reader.get(r) != Integer.MAX_VALUE && reader.get(r) < target) {
            l = r;
            r *= 2;
        }
        while (l <= r) {
            int mid = l + (r - l) / 2;
            if (reader.get(mid) == Integer.MAX_VALUE) {
                r = mid - 1;
            } else if (reader.get(mid) == target) {
                return mid;
            } else if (reader.get(mid) < target) {
                l = mid + 1;
            } else {
                r = mid - 1;
            }
        }
        return -1;
    }
}
```