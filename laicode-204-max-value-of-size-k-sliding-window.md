# Laicode 204. Maximum Values Of Size K Sliding Windows

参照[leetcode 239](239-Sliding-Window-Maximum.md)。

```java
public class Solution {
  public List<Integer> maxWindows(int[] array, int k) {
    Deque<Integer> deque = new ArrayDeque<>();
    List<Integer> res = new ArrayList<>();
    for(int i = 0; i < array.length; i++) {
      while(!deque.isEmpty() && deque.peekFirst() < i - k + 1) {
        // remove all indexs left to left boundary
        deque.pollFirst();
      }
      while(!deque.isEmpty() && array[deque.peekLast()] < array[i]) {
        deque.pollLast();
      }
      deque.offerLast(i);
      if(i >= k - 1) {
        res.add(array[deque.peekFirst()]);
      }
    }
    return res;
  }
}
```