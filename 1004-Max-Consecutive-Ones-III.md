# 1004. Max Consecutive Ones III

给定一个只有0和1组成的数组，允许你最多把K个0变成1，找到最长的连续1子数组长度。

允许最多把K个0变成1，那也其实就是说找到最长的连续1的子数组，它里面最多可以有K个0。那么这题其实还是滑动窗口的问题。

`l` 记录滑动窗口的起始位置，每轮循环结束时我都要`[l, i]`里面不超过K个0。`count0`用来记录当前窗口里面 0 的个数，如果`nums[i]`是0的话，`count0`就自增。下面开始维护，如果`count0`已经比k大了，那么 l 就往后一直走，路过 `nums[l] == 0` 的时候，count0 可以减一个，走到count0不大于k为止。此时，我们记录当前窗口的长度并且和答案对比试图更新答案。

Time complexity: O(N), we access each character at most twice.

Space complexity: O(1).

```java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int count0 = 0;
        int l = 0;
        int longest = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) {
                count0++;
            }
            while (count0 > k) {
                if (nums[l] == 0) {
                    count0--;
                }
                l++;
            }
            longest = Math.max(longest, i - l + 1);
        }
        return longest;
    }
}
```