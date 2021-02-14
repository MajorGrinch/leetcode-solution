# Laicode 25. K Smallest In Unsorted Array

## Max Heap Approach

这题可以采用大顶堆的方法，当大顶堆内元素不足k个的时候持续放入，当大顶堆内元素已经k个了就看看堆顶那个最大的元素和当前元素比谁小，当前元素小的话就弹出堆顶元素，放入新元素。这样我们就可以保持堆内始终放着最小的k个元素。

当然了，这题需要用到大顶堆。一般我们是用Java的`PriorityQueue`类，但是呢这里我们要深入理解堆操作的原理，所以自己实现一个大顶堆。大顶堆放入新元素的时间复杂度是O(logN)，弹出堆顶时间复杂度也是O(logN)，这里的N是堆的大小。所以整个流程下来，时间复杂度是O(Nlogk)，空间复杂度是O(k)，这里的N是输入数组的大小。

Time complexity: O(Nlogk)

Space complexity: O(k)

```java
public class Solution {
  public int[] kSmallest(int[] array, int k) {
    if(array == null || array.length == 0 || k == 0){
      return new int[0];
    }
    MaxHeap maxHeap = new MaxHeap(k);
    for(int num: array){
      if(maxHeap.getCount() >= k){
        if(maxHeap.peek() > num){
          maxHeap.poll();
          maxHeap.offer(num);
        }
      }else{
        maxHeap.offer(num);
      }
    }
    int[] res = new int[k];
    for(int i = k - 1; i >= 0; i--){
      res[i] = maxHeap.poll();
    }
    return res;
  }
}

class MaxHeap{

  private int[] array;

  int count = 0;

  MaxHeap(int size){
    // choose 1 as start, children are 2k and 2k + 1 respectively.
    this.array = new int[size + 1];
  }

  public int getCount(){
    return count;
  }

  public void offer(int num){
    array[++count] = num;
    swim(count);
  }

  public int poll(){
    int res = array[1];
    swap(array, 1, count--);
    sink(1);
    return res;
  }

  public int peek(){
    return array[1];
  }

  private void sink(int k){
    while(2 * k <= count){
      int c = 2 * k;
      if(c < count && array[c] < array[c + 1]){
        // choose the bigger child
        c++;
      }
      if(array[c] < array[k]){
        // bigger child is less than current node
        break;
      }
      swap(array, k, c);
      k = c;
    }
  }

  private void swim(int k){
    while(k > 1 && array[k] > array[k/2]){
      swap(array, k, k/2);
      k /= 2;
    }
  }

  private void swap(int[] array, int i, int j){
    int temp = array[i];
    array[i] = array[j];
    array[j] = temp;
  }
}
```