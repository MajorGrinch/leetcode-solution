# 80. Remove Duplicates from Sorted Array II

给定一个升序数组，虽然也是要求去重，不过重复元素可以保留2个。要求在原数组上改动，并且返回最后还剩多少元素。

这题是[26. Remove Duplicates from Sorted Array](26-Remove-Duplicates-From-Sorted-Array.md)的延伸题型，依然是快慢指针。但是这次我们允许最多重复2次，所以和26题略微不同的地方在于我们检查`slow - 2`和`i`对应的元素。`slow - 1`我根本不关心，就算和`i`对应的元素一样，那也无所谓。控制好`slow - 2`和`i`不一样就可以保证`[0, slow)`没有超过2个重复的元素。

那么我们思考一下，以后遇到这个题型，如果是最多保留k个，那就是控制`slow - k`和`i`不一样就好了。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
  public int removeDuplicates(int[] nums) {
    if (nums.length <= 2) {
      return nums.length;
    }
    int slow = 2;
    for (int i = 2; i < nums.length; i++) {
      if (nums[i] != nums[slow - 2]) {
        nums[slow++] = nums[i];
      }
    }
    return slow;
  }
}
```