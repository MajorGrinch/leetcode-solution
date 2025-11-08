# Laicode 113. Median Tracker

参照[leetcode 295](295-Find-Median-from-Data-Stream.md)。

```java
public class Solution {
  private PriorityQueue<Integer> minHeap;
  private PriorityQueue<Integer> maxHeap;

  public Solution() {
    minHeap = new PriorityQueue<>();
    maxHeap = new PriorityQueue<>(Collections.reverseOrder());
  }
  
  public void read(int value) {
    minHeap.offer(value);
    maxHeap.offer(minHeap.poll());
    if(maxHeap.size() - minHeap.size() > 1) {
      minHeap.offer(maxHeap.poll());
    }
  }
  
  public Double median() {
    if(minHeap.isEmpty() && maxHeap.isEmpty()) {
      return null;
    }
    if(minHeap.size() == maxHeap.size()) {
      return (minHeap.peek() + maxHeap.peek()) / 2.0;
    } else {
      return maxHeap.peek() * 1.0;
    }
  }
}
```