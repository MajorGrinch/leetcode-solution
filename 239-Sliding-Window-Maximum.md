# 239. Sliding Window Maximum

给定一个数组，和一个长度为k的sliding window在数组里从左滑到右。求出滑动过程中，sliding window所有的最大值。

这题我们用一个双端队列，来依次存放滑动窗口里所有元素的下标。遍历数组，当前下标 `i`作为窗口的右端，当前窗口就是 `[i - k + 1, i]`。每当到一个新的 `i`的时候，窗口里从右往左开始所有比它小的下标，都没有必要继续在窗口里了，因为当前窗口的最大值怎么也轮不到它们了。所以每当sliding window向右滑动，新的元素进来的时候，把双端队列右部所有小于等于新元素的下标都清除，保持双端队列里面从左到右下标对应的元素呈降序。滑动窗口里的最大值的下标就是双端队列里的头部元素。当然，滑动之后也要看看队列头部的那些下标是否还在窗口里，不在的话就要踢出去。

Time complexity: O(n) because each index is pushed and popped at most once.

Space complexity: O(k)

```java
class Solution {
  public int[] maxSlidingWindow(int[] nums, int k) {
    Deque<Integer> window = new ArrayDeque<>();
    int n = nums.length;
    int[] res = new int[n - k + 1];
    // let i be the window's right boundary
    for (int i = 0; i < n; i++) {
      while (!window.isEmpty() && window.peekFirst() < i - k + 1) {
        // remove all indexes out of left boundary
        window.pollFirst();
      }
      while (!window.isEmpty() && nums[window.peekLast()] < nums[i]) {
        // remove all indexes of elements that are smaller than current one
        window.pollLast();
      }
      window.offerLast(i);
      if (i >= k - 1) {
        res[i - k + 1] = nums[window.peekFirst()];
      }
    }
    return res;
  }
}
```