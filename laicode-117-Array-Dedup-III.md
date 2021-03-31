# Laicode 117. Array Deduplication III

和前面两题差不多，只不过这次是不保留任何重复元素。

这次我们需要重新审视一下这题，`[0, slow)`依然存放去重之后的元素。但是这题不能像前两题一样实现了，这题我们需要跳过整段的重复元素。所以快指针在往后走的时候需要用一个begin来记录起始位置，然后跳过所有是重复的元素。只有遇见非重复元素时候，才加入`[0, slow)`。

Time complexity: O(N)

Space complexity: O(1)，如果返回答案不算的话。

```java
public class Solution {
  public int[] dedup(int[] array) {
    if (array == null || array.length <= 1) {
      return array;
    }
    int slow = 0, fast = 0;
    while (fast < array.length) {
      int begin = fast;
      while (fast < array.length && array[fast] == array[begin]) fast++;

      if (fast - begin == 1) array[slow++] = array[begin];
    }
    int[] res = new int[slow];
    for (int i = 0; i < slow; i++) res[i] = array[i];
    return res;
  }
}
```