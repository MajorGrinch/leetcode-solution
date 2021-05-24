# Laicode 25. K Smallest In Unsorted Array

## Max Heap Approach

这题可以采用大顶堆的方法。当大顶堆内元素不足k个的时候持续放入，当大顶堆内元素已经大于k个了就弹出堆顶最大的那个，这样我们就可以保持堆内始终放着最小的k个元素。不过这也要求我们大顶堆建立的时候`maxSize = k + 1`来让我们能放`k+1`个元素进去，来获得比较的余地。

当然了，这题需要用到大顶堆。一般我们是用Java的`PriorityQueue`类，但是呢这里我们要深入理解堆操作的原理，所以自己实现一个大顶堆。大顶堆放入新元素的时间复杂度是O(logN)，弹出堆顶时间复杂度也是O(logN)，这里的N是堆的大小。所以整个流程下来，时间复杂度是O(Nlogk)，空间复杂度是O(k)，这里的N是输入数组的大小。

Time complexity: O(Nlogk)

Space complexity: O(k)

```java
public class Solution {
  public int[] kSmallest(int[] array, int k) {
    if (array == null || array.length == 0 || k == 0) {
      return new int[0];
    }
    MaxHeap maxHeap = new MaxHeap(k + 1);
    for (int num : array) {
      maxHeap.offer(num);
      if (maxHeap.size() > k) maxHeap.poll();
    }
    int[] res = new int[k];
    for (int i = k - 1; i >= 0; i--) {
      res[i] = maxHeap.poll();
    }
    return res;
  }
}

class MaxHeap {

  private int[] array;

  int count = 0;

  MaxHeap(int maxSize) {
    // choose 1 as start, children are 2k and 2k + 1 respectively.
    this.array = new int[maxSize + 1];
  }

  public int size() {
    return count;
  }

  public void offer(int num) {
    array[++count] = num;
    swim(count);
  }

  public int poll() {
    int res = array[1];
    swap(array, 1, count--);
    sink(1);
    return res;
  }

  private void sink(int k) {
    while (2 * k <= count) {
      int c = 2 * k;
      if (c < count && array[c] < array[c + 1]) {
        // choose the bigger child
        c++;
      }
      if (array[c] < array[k]) {
        // bigger child is less than current node
        break;
      }
      swap(array, k, c);
      k = c;
    }
  }

  private void swim(int k) {
    while (k > 1 && array[k] > array[k / 2]) {
      swap(array, k, k / 2);
      k /= 2;
    }
  }

  private void swap(int[] array, int i, int j) {
    int temp = array[i];
    array[i] = array[j];
    array[j] = temp;
  }
}
```