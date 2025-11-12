# 485. Max Consecutive Ones

## DP Approach

参照[laicode 103](laicode-103-Longest-Consecutive-Ones.md)。

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int[] ones = new int[nums.length];
        ones[0] = nums[0];
        int res = ones[0];
        for(int i = 1; i < ones.length; i++) {
            ones[i] = nums[i] == 1 ? ones[i - 1] + 1 : 0;
            res = Math.max(res, ones[i]);
        }
        return res;
    }
}
```

## Greedy Approach

curr记录以 i-1 为末尾的全 1 子数组长度，res 记录全局最大值，i 从 0 遍历到末尾。那么很显然，cur 初始化为 0，res也是 0。

当前 nums[i]为 1 的话，那么以 i 结尾的全 1 子数组长度就取决于以 i-1 结尾的全 1 子数组长度加一。当前 nums[i]为 0 的话，那么以 i 结尾的全 1 子数组长度就是 0。每轮循环结束时候，更新全局答案。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int curr = 0;
        int res = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 1) {
                curr += 1;
            } else {
                curr = 0;
            }
            res = Math.max(res, curr);
        }
        return res;
    }
}
```