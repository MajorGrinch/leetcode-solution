# Laicode 118. Array Deduplication IV

给定一个有重复元素的数组，不断地去掉所有相邻的重复元素，直到没有相邻的重复元素为止。比如`[1,2,3,3,3,2,2]` -> `[1,2,2,2]` -> `[1]`。

那么这题依然是双指针，`[0, slow)`记录着去重之后的部分，`fast`指向当前要处理的元素。因为连续的重复元素一个不留，所以我们依然用一个`begin`来记录`fast`初始位置，然后`fast`向后走到和`begin`指向的元素不同为止。如果`[0, slow)`不为空而且`array[begin] == array[slow - 1]`，那么`arra[slow - 1]`和`array[begin, fast)`都不能留。否则，如果`fast - begin == 1`也就意味着当前元素本身没有相邻重复元素，而且加入`[0, slow)`之后也不会和之前的元素重复，所以就把`array[begin]`加入`[0, slow)`。

Time complexity: O(N)

Space complexity: O(1)，如果答案不算入的话。

```java
public class Solution {
  public int[] dedup(int[] array) {
    if (array.length <= 1) {
      return array;
    }
    int slow = 0, fast = 0;
    while (fast < array.length) {
      int begin = fast;
      while (fast < array.length && array[fast] == array[begin]) fast++;
      if (slow > 0 && array[begin] == array[slow - 1]) {
        slow--;
      } else if (fast - begin == 1) {
        array[slow++] = array[begin];
      }
    }
    int[] res = new int[slow];
    for (int i = 0; i < slow; i++) res[i] = array[i];
    return res;
  }
}
```