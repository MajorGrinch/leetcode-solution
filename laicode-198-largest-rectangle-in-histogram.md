# Laicode 198. Largest Rectangle In Histogram



```java
public class Solution {
  public int largest(int[] array) {
    Deque<Integer> stack = new ArrayDeque<>();
    int res = 0;
    int[] histogram = new int[array.length + 2];
    histogram[0] = -1;
    for(int i = 0; i < array.length; i++) {
      histogram[i + 1] = array[i];
    }
    histogram[histogram.length - 1] = 0;
    for(int i = 0; i < histogram.length; i++) {
      while(!stack.isEmpty() && histogram[stack.peek()] >= histogram[i]) {
        int h = histogram[stack.pop()];
        int l = i - 1 - stack.peek();
        int area = h * l;
        res = Math.max(res, area);
      }
      stack.push(i);
    }
    return res;
  }
}
```