# 658. Find K Closest Elements

这题依然是二分搜索找到第一个大于等于`x`的的元素，以它为原点向前取k个元素，向后取k-1个元素。这样我就可以保证，最接近x的k个元素一定在这个2k个元素的区间里。

不过需要注意的是，向前不一定有k个，向后也不一定有k-1个。因为数组长度都不一定有2k，所以注意边界检查。

从两头开始比较，距离x远的那个就逐步排除出去。不过注意，题目规定了如果`a`和`b`距离x一样，我们认为小的那个更近。

Corner case:
+ 数组本身不足k+1个元素，则取全体元素返回

```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        if(arr == null || arr.length == 0 || k == 0){
            return new ArrayList<>();
        }
        List<Integer> res = new ArrayList<>();
        if(arr.length <= k){
            for(int a: arr){
                res.add(a);
            }
            return res;
        }

        int firstGEIndex = firstGE(arr, x);
        int low = Math.max(0, firstGEIndex - k);
        int high = Math.min(arr.length - 1, firstGEIndex + k - 1);
        while(high - low + 1> k){
            if(x - arr[low] > arr[high] - x){
                low++;
            }else{
                high--;
            }
        }
        for(int i = low; i <= high; i++){
            res.add(arr[i]);
        }
        return res;
    }

    /**
     * Find first element greater than or equal to x.
     */
    private int firstGE(int[] arr, int x){
        int l = 0, r = arr.length - 1;
        while(l < r){
            int mid = l + (r-l >> 1);
            if(arr[mid] < x){
                l = mid + 1;
            }else{
                r = mid;
            }
        }
        return l;
    }
}
```