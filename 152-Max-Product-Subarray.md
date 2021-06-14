# 152. Maximum Product Subarray

给定一个数组，求出乘积最大的子数组的，返回对应的乘积。

因为说了是子数组，所以肯定是连续的，那么其实这题和[53. Maximum Subarray](53-Maximum-Subarray.md)类似。只不过这题是求乘积，不同的地方在于乘积我们截断的条件是当前乘积小于1，和截断的条件是当前和小于0。而且，乘积有另一个特性就是一个很小的负数乘以负数就可以负负得正。目前的最小乘积乘个负数也许就变成了最大乘积。所以，我们沿途不仅要记录目前的最大值，也要记录目前的最小值。每次都从`nums[i], currMax * nums[i], currMin * nums[i]`里面挑选最大的成为目前的全局最大值`currMax`以及最小的成为目前的全局最小值`currMin`。

Time complexity: O(n)

Space complexity: O(1)

```java
class Solution {
  public int maxProduct(int[] nums) {
    if (nums.length == 1) {
      return nums[0];
    }

    int currMax = nums[0], currMin = nums[0];
    int globalMax = nums[0];
    for (int i = 1; i < nums.length; i++) {
      int temp = currMax;
      currMax = Math.max(nums[i], Math.max(currMax * nums[i], currMin * nums[i]));
      currMin = Math.min(nums[i], Math.min(temp * nums[i], currMin * nums[i]));
      globalMax = Math.max(globalMax, currMax);
    }
    return globalMax;
  }
}
```