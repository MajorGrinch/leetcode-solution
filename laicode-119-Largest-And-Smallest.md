# Laicode 119. Largest And Smallest

给定一个无序数组，用最小的比较次数找出最大和最小值。

看到这题，找最大最小很简单，一次遍历两个变量比较就好。但是这个方法需要2N次比较，题目既然说了要用最少的比较次数，那么说明肯定有比2N次更少的。

我们可以双指针从两头同时往中间走，每次遇到`array[l] > array[r]`就把两个对调。这样我们可以保证前面一半里面每个下标i元素，都比`n - 1 -i`要小。那么最小元素一定在前面一半里，最大元素一定在后面一半里。前后一半各自找最大最小，只需要1.5N次的比较就好。

Time complexity: O(N)

Space complexity: O(1)

```java
public class Solution {
  public int[] largestAndSmallest(int[] array) {
    int l = 0, r = array.length - 1;
    while (l < r) {
      if (array[l] > array[r]) {
        swap(array, l, r);
      }
      l++;
      r--;
    }
    int[] res = new int[2];
    res[0] = getLargest(array, r, array.length - 1);
    res[1] = getSmallest(array, 0, l);
    return res;
  }

  private int getSmallest(int[] array, int left, int right) {
    int smallest = Integer.MAX_VALUE;
    for (int i = left; i <= right; i++) {
      if (smallest > array[i]) {
        smallest = array[i];
      }
    }
    return smallest;
  }

  private int getLargest(int[] array, int left, int right) {
    int largest = Integer.MIN_VALUE;
    for (int i = left; i <= right; i++) {
      if (largest < array[i]) {
        largest = array[i];
      }
    }
    return largest;
  }

  private void swap(int[] array, int i, int j) {
    int temp = array[i];
    array[i] = array[j];
    array[j] = temp;
  }
}
```