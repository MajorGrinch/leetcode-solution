# 209. Minimum Size Subarray Sum

题目要求找出元素和大于等于 target 的最短子数组的长度，如果没有就返回 0。

那么这题我们用滑动窗口的思想，currSum记录当前滑动窗口的元素和，currLeft 记录当前滑动窗口的左边界。每遍历一个元素的时候，当前元素加入 currSum，一旦 currSum >= target 就尝试更新最短子数组的长度，并且尽力把子数组缩到最短。最后，我们就可以得到最短满足条件的子数组长度。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int currSum = 0;
        int currLeft = 0;
        int res = nums.length + 1;
        for (int i = 0; i < nums.length; i++) {
            currSum += nums[i];
            while (currSum >= target) {
                res = Math.min(res, i - currLeft + 1);
                currSum -= nums[currLeft++];
            }
        }
        return res < nums.length + 1 ? res : 0;
    }
}
```