# 702. Search in a Sorted Array of Unknown Size

这题主要是array长度未知，所以我们要先确定二分搜索的左右边界。

先确定左右边界为0和1，不断地对对范围进行double来确定target是否在[l, r]之中。

敲定了target所在的范围之后，对这个范围进行二分搜索。

二分搜索的循环不变量是，target始终在[l, r]这个区间里。如果能找得到，那么一定会在循环结束之前找到并返回。如果循环顺利结束，l > r，说明没找到。

```java
/**
 * // This is ArrayReader's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface ArrayReader {
 *     public int get(int index) {}
 * }
 */

class Solution {
    public int search(ArrayReader reader, int target) {
        if(reader == null){
            return -1;
        }
        if(reader.get(0) >= 10000){
            return -1;
        }
        int l = 0, r = 1;
        while(reader.get(r) < target){
            l = r;
            r *= 2;
        }
        return binarySearch(reader, l, r, target);
    }

    private int binarySearch(ArrayReader reader, int lb, int rb, int target){
        int l = lb, r = rb;
        while(l <= r){
            int mid = l + (r-l >>1);
            int midV = reader.get(mid);
            if(midV == target){
                return mid;
            }else if(midV < target){
                l = mid + 1;
            }else{
                r = mid - 1;
            }
        }
        return -1;
    }
}
```