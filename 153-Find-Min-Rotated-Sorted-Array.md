# 153. Find Minimum in Rotated Sorted Array

给定被循环右移过后的一个升序数组，找到最小的数。

Clarification:
+ 无重复元素

这题还是二分搜索，我们知道最小元素肯定是断崖右边那个元素，所以我们取整个数组最后一个元素作为支点来比较。如果`nums[mid] > pivot`说明`mid`在断崖左边，`l = mid + 1`。如果`nums[mid] < pivot`，说明`mid`在断崖右边但是我们并不知道是不是紧挨着断崖，所以`r = mid`。`nums[mid] == pivot`，看clarification就知道。除非循环右移n-1次，而且即便这样搜索过程中也不会出现这个情况。

那么我们为什么取最后一个元素作为支点？第一个元素不行么？他们作为本来的邻居，一个是左半边最小，另一个是右半边最大。为什么非要用最后一个元素？这里涉及到一个情况，没有断崖怎么办？假如循环右移N次，相当于没动。那么这种情况，我们的二分搜索就很被动。如果用首个元素作为支点，这种情况需要额外判断。但是如果用最后一个元素作为支点，我们知道最小点一定是在这个点的左边或者是他自己本身，不用考虑到底有没有断崖。有没有断崖，我们都可以按照现有逻辑来对`r`进行移动以找最小点。

Time complexity: O(logn)

Space complexity: O(1)

```java
class Solution {
  public int findMin(int[] nums) {
    int l = 0, r = nums.length - 1;
    int pivot = nums[r];
    while (l < r) {
      int mid = l + (r - l >> 1);
      if (nums[mid] > pivot) {
        l = mid + 1;
      } else {
        r = mid;
      }
    }
    return nums[l];
  }
}
```