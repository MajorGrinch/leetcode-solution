# 259. 3Sum Smaller

给定一个无序数组，找出所有和小于 target 的三元组，返回三元组的个数即可。

那么我们给数组先排个序，然后遍历 `[0, n - 3]`这些元素，对于每一个 `nums[i]`，我们可以看看 `[i + 1, n - 1]` 里面有多少个二元组和是小于 `target - nums[i]` 的就好，累加起来就可以得到答案。



```java
class Solution {
    public int threeSumSmaller(int[] nums, int target) {
        if (nums == null || nums.length <= 2) {
            return 0;
        }
        Arrays.sort(nums);
        int res = 0;
        for (int i = 0; i < nums.length - 2; i++) {
            res += twoSumSmaller(nums, i + 1, nums.length - 1, target - nums[i]);
        }
        return res;
    }

    private int twoSumSmaller(int[] nums, int l, int r, int target) {
        int count = 0;
        while (l < r) {
            int sum = nums[l] + nums[r];
            if (sum < target) {
                count += r - l;
                l++;
            } else {
                r--;
            }
        }
        return count;
    }
}
```