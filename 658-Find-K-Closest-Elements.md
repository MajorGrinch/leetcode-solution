# 658. Find K Closest Elements

这题依然是二分搜索找到第一个大于等于`x`的的元素，以它为原点向前取k个元素，向后取k-1个元素。这样我就可以保证，最接近x的k个元素一定在这个2k个元素的区间里。

不过需要注意的是，向前不一定有k个，向后也不一定有k-1个。因为数组长度都不一定有2k，所以注意边界检查。

从两头开始比较，距离x远的那个就逐步排除出去。不过注意，题目规定了如果`a`和`b`距离x一样，我们认为小的那个更近。

Corner case:
+ 数组本身不足k+1个元素，则取全体元素返回

Time complexity: O(logN + k), N is array length.

Space complexity: O(k), because we have to return list.

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

2025 update:
```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        if (arr == null || arr.length == 0 || k == 0) {
            return new ArrayList<>();
        }
        List<Integer> res = new ArrayList<>();

        if (arr.length <= k) {
            for (int i = 0; i < arr.length; i++)
                res.add(arr[i]);
            return res;
        }

        if (x <= arr[0]) {
            for (int i = 0; i < k; i++)
                res.add(arr[i]);
        } else if (arr[arr.length - 1] <= x) {
            for (int i = arr.length - k; i < arr.length; i++)
                res.add(arr[i]);
        } else {
            int pos = firstGE(arr, x);
            int low = Math.max(0, pos - k);
            int high = Math.min(arr.length - 1, pos + k - 1);
            while (high - low > k - 1) {
                if (Math.abs(arr[low] - x) <= Math.abs(arr[high] - x)) {
                    high--;
                } else {
                    low++;
                }
            }
            for (int i = low; i <= high; i++)
                res.add(arr[i]);
        }
        return res;
    }

    private int firstGE(int[] arr, int x) {
        int l = 0;
        int r = arr.length - 1;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (x <= arr[mid]) {
                r = mid;
            } else {
                // nums[mid] < target
                l = mid + 1;
            }
        }
        return l;
    }
}
```

也可以这么写，其实就是跳过了 `x <= arr[0]` 和 `arr[arr.length - 1] <= x` 这两种情况的检验，因为后续也能处理好。代码更少，更简洁一些。

```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        int firstGEIdx = firstGE(arr, x);
        int l = firstGEIdx - 1;
        int r = firstGEIdx;
        while (r - l - 1 < k) {
            if (l < 0) {
                r++;
            } else if (r >= arr.length) {
                l--;
            } else if (Math.abs(arr[l] - x) > Math.abs(arr[r] - x)) {
                r++;
            } else {
                l--;
            }
        }
        List<Integer> res = new ArrayList<>();
        for (int i = l + 1; i < r; i++) {
            res.add(arr[i]);
        }
        return res;
    }

    private int firstGE(int[] arr, int target) {
        int l = 0;
        int r = arr.length - 1;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (arr[mid] < target) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        return l;
    }
}
```