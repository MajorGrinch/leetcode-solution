# Laicode 276. Get Count Array

参考[leetcode 315](315-count-of-smaller-numbers-after-self.md)。

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