# 26. Remove Duplicates from Sorted Array

给定一个升序数组，在原数组内部清除所有重复元素，返回清除后还剩多少个元素。

这个其实和[laicode 79. Remove Adjacent Repeated Characters I](laicode-79-Remove-Adjacent-Repeated-Chars-I.md)差不多思路，也是快慢指针。`[0, slow)`记录着`[0, i)`去重之后的元素，如果目前`i`对应的元素和`slow - 1`对应的元素不一样的话，就加入，否则就跳过。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
  public int removeDuplicates(int[] nums) {
    if (nums.length <= 1) {
      return nums.length;
    }
    int slow = 1;
    for (int i = 1; i < nums.length; i++) {
      if (nums[i] != nums[slow - 1]) {
        nums[slow++] = nums[i];
      }
    }
    return slow;
  }
}
```

2025 update:

通用解法

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int slow = 0;
        int fast = 0;
        while(fast < nums.length) {
            int begin = fast;
            while(fast < nums.length && nums[fast] == nums[begin]) {
                fast++;
            }
            nums[slow++] = nums[begin];
        }
        return slow;
    }
}
```