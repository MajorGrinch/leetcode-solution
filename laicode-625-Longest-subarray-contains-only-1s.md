# Laicode 625. Longest subarray contains only 1s

给定一个int数组，只含有0和1，可以把k个0翻成1。求最长的全是1的子数组。

这题开始快慢双指针的思想，每轮循环的不变量 -- [st, i]存放的是反转K次0的全1子数组。

那么我们需要记录遇到0的次数，每当遇到0的时候，我们就需要加一次。如果0的次数突破了K，那么我们的st指针就需要后移，一直移到0的次数等于K为止。这样每轮循环结束的时候，我们都可以说 [st, i] 是全1子数组。记录一个全局最长的子数组长度，不断地更新就得到了最后的答案。

```java
public class Solution {
  public int longestConsecutiveOnes(int[] nums, int k) {
    if(nums == null || nums.length == 0) {
      return 0;
    }
    int st = 0;
    int res = 0;
    int count0 = 0;
    for(int i = 0; i < nums.length; i++) {
      if(nums[i] == 0) {
        count0++;
        while(count0 > k && st <= i) {
          if(nums[st] == 0) {
            count0--;
          }
          st++;
        }
      }
      res = Math.max(res, i - st + 1);
    }
    return res;
  }
}
```