# 674. Longest Continuous Increasing Subsequence

给定一个数组，找到最长升序子数组的长度并返回。

非常简单的动态规划题。我们可以计算出给定任意一个下标`i`，以它结尾的升序子数组长度是多少，放在`lis[i]`里。那么`lis[i]`其实某种程度上来说就是取决于`lis[i-1]`。如果`nums[i]`比`nums[i-1]`大的话，那么`lis[i]`就是`lis[i] + 1`。如果`nums[i]`不比`nums[i-1]`大的话，那么`lis[i]`就重置为1，因为以`i`结尾的升序子数组就只有`i`一个元素。

用lisLength来放答案，初始化`lis[0]`为1，然后从下标1开始遍历到最后，途中不断计算`lis[i]`并更新lisLength。最后，我们就可以得到答案。

Time complexity: O(N)

Space complexity: O(N)

```java
class Solution {
  public int findLengthOfLCIS(int[] nums) {
    if (nums == null || nums.length == 0) {
      return 0;
    }
    int[] lis = new int[nums.length];
    lis[0] = 1;
    int lisLength = 1;
    for (int i = 1; i < nums.length; i++) {
      if (nums[i] > nums[i - 1]) {
        lis[i] = lis[i - 1] + 1;
      } else {
        lis[i] = 1;
      }
      lisLength = Math.max(lisLength, lis[i]);
    }
    return lisLength;
  }
}
```

2025 code:

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int n = nums.length;
        if (n <= 1) {
            return n;
        }
        int[] increasing = new int[n];
        increasing[0] = 1;
        int res = 1;
        for (int i = 1; i < n; i++) {
            if (nums[i] > nums[i - 1]) {
                increasing[i] = increasing[i - 1] + 1;
            } else {
                increasing[i] = 1;
            }
            res = Math.max(res, increasing[i]);
        }
        return res;
    }
}
```