# Laicode 625. Longest subarray contains only 1s

参照[leetcode 1004](1004-Max-Consecutive-Ones-III.md)。

```java
public class Solution {
  public int longestConsecutiveOnes(int[] nums, int k) {
    int st = 0;
    int count0 = 0;
    int longest = 0;
    for(int i = 0; i < nums.length; i++) {
      if(nums[i] == 0) {
        count0++;
      }
      while(count0 > k) {
        if(nums[st] == 0) {
          count0--;
        }
        st++;
      }
      longest = Math.max(longest, i - st + 1);
    }
    return longest;
  }
}

```