# 315. Count of Smaller Numbers After Self

给定一个无序数组，返回每个元素右边比它小的元素个数。这题有点难，不想做可以跳过。

这题我们可以用merge sort的思想来解决，merge sort在merge的过程中会把前后两半有序的给合并成整体有序，那么其实在这个过程中我们可以顺带记录下每个元素右边到底有多少个比它小的元素。那么到底如何记录呢，其实也简单。当我们某一轮是选中左半边某个元素时，那么此时`[mid + 1, j)`的所有元素其实都是在该元素右边但是却比它小的。有一个需要注意的地方，那就是merge 的时候当两个指针 i 和 j 指的元素相等时，该选哪一个？应该选前面那个i 的，因为如果选 j 的话，我们移动了 j，那么我们后面移动 i 的时候就不能说 `[mid + 1, j)` 都比 i 小了。

话又说回来，我们还不能真的对这个数组进行sort。因为sort之后，顺序就没法保持了。所以我们其实是要对数组下标进行sort，sort的条件就是下标对应元素小的放在前面。用rightSmaller数组来记录每个下标右边到底有多少个元素比自己小。

Time complexity: O(NlogN)

Space complexity: O(N)

```java
class Solution {
    public List<Integer> countSmaller(int[] nums) {
        int n = nums.length;
        int[] rightSmaller = new int[n];
        int[] index = new int[n];
        for (int i = 0; i < n; i++) {
            index[i] = i;
        }
        mergeSort(index, 0, nums.length - 1, nums, new int[nums.length], rightSmaller);
        return Arrays.stream(rightSmaller).boxed().toList();
    }

    private void mergeSort(int[] index, int l, int r, int[] nums, int[] aux, int[] rightSmaller) {
        if (l >= r) {
            return;
        }
        int mid = l + (r - l) / 2;
        mergeSort(index, l, mid, nums, aux, rightSmaller);
        mergeSort(index, mid + 1, r, nums, aux, rightSmaller);
        merge(index, l, mid, r, nums, aux, rightSmaller);
    }

    private void merge(int[] index, int l, int mid, int r, int[] nums, int[] aux, int[] rightSmaller) {
        for (int k = l; k <= r; k++) {
            aux[k] = index[k];
        }
        int i = l;
        int j = mid + 1;
        for (int k = l; k <= r; k++) {
            if (i > mid) {
                index[k] = aux[j++];
            } else if (j > r) {
                index[k] = aux[i++];
                rightSmaller[index[k]] += r - mid;
            } else if (nums[aux[i]] <= nums[aux[j]]) {
                index[k] = aux[i++];
                rightSmaller[index[k]] += j - mid - 1;
            } else {
                index[k] = aux[j++];
            }
        }
    }
}
```