# Laicode 276. Get Count Array

给定一个无序数组，返回每个元素右边比它小的元素个数。这题有点难，不想做可以跳过。

这题我们可以用merge sort的思想来解决，merge sort在merge的过程中会把前后两半有序的给合并成整体有序，那么其实在这个过程中我们可以顺带记录下每个元素右边到底有多少个比它小的元素。那么到底如何记录呢，其实也简单。当我们某一轮是选中左半边某个元素时，那么此时`[mid + 1, j)`的所有元素其实都是在该元素右边但是却比它小的。

话又说回来，我们还不能真的对这个数组进行sort。因为sort之后，顺序就没法保持了。所以我们其实是要对数组下标进行sort，sort的条件就是下标对应元素小的放在前面。用rightSmaller数组来记录每个下标右边到底有多少个元素比自己小。

Time complexity: O(NlogN)

Space complexity: O(N)

```java
public class Solution {
  public int[] countArray(int[] array) {
    if (array.length == 0) {
      return array;
    }
    int n = array.length;
    int[] indexes = generateIndexArray(n);
    int[] rightSmaller = new int[n];
    mergeSort(indexes, 0, n - 1, array, new int[n], rightSmaller);
    return rightSmaller;
  }

  private void mergeSort(
      int[] indexes, int left, int right, int[] array, int[] aux, int[] rightSmaller) {
    if (left >= right) {
      return;
    }
    int mid = left + (right - left >> 1);
    mergeSort(indexes, left, mid, array, aux, rightSmaller);
    mergeSort(indexes, mid + 1, right, array, aux, rightSmaller);
    merge(indexes, left, mid, right, array, aux, rightSmaller);
  }

  private void merge(
      int[] indexes, int left, int mid, int right, int[] array, int[] aux, int[] rightSmaller) {
    for (int i = left; i <= right; i++) {
      aux[i] = indexes[i];
    }
    int i = left, j = mid + 1;
    for (int k = left; k <= right; k++) {
      if (i > mid) {
        indexes[k] = aux[j++];
      } else if (j > right) {
        indexes[k] = aux[i++];
        rightSmaller[indexes[k]] += j - mid - 1;
      } else if (array[aux[i]] <= array[aux[j]]) {
        indexes[k] = aux[i++];
        rightSmaller[indexes[k]] += j - mid - 1;
      } else {
        indexes[k] = aux[j++];
      }
    }
  }

  private int[] generateIndexArray(int n) {
    int[] indexes = new int[n];
    for (int i = 0; i < n; i++) indexes[i] = i;
    return indexes;
  }
}
```