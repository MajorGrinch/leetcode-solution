# 410. Split Array Largest Sum

## DP Solution

这题可以用动态规划的思想来解决，把大问题分解成一个个子问题，先把子问题的答案解出来，最后得出总问题的答案。

创建一个二维数组 res，res[i][j] 的含义就是 `[0, i)` 数组分成 `j` 个子数组的答案。比如，`res[5][3]` 含义就是 `[0, 5)` 子数组如果分 3 段的答案是多少。那么最后总问题的答案就是 `res[n][k]`，所以二维数组是 `new int[n + 1][k + 1]`。

既然动态规划了，那么状态转移方程是什么呢？对于一个 res[i][j]，我们可以知道它无非就是这样分：
+ res[j - 1][j - 1], nums[j - 1, i)
+ res[j][j - 1], nums[j, i)
+ res[j + 1][j - 1], nums[j + 1, i)
+ ...
+ res[i - 2][j - 1], nums[i - 2, i)
+ res[i - 1][j - 1], nums[i - 1, i)

为什么从 `res[j - 1][j - 1]` 开始？因为只有 `nums[0, j - 1)` 可以分成 `j - 1`段子数组。那么 `res[i][j]` 很明显来自于上面所有情况里最小的那个组合。

关于 res 的初始化，很显然，任意 `res[0][j]` 都是0。然后从 `res[1]` 行开始之后的所有行，都初始化为最大 int 值。

为了快速得到特定区间子数组的和，我们需要一个前缀和数组 `prefixSum[i] ==> nums[0, i)`。

最后答案就是 `res[n][k]`。

Time complexity: O(N * N * K)

Space complexity: O(N * K)

```java
class Solution {
    public int splitArray(int[] nums, int k) {
        int n = nums.length;
        int[][] res = new int[n + 1][k + 1];
        int[] prefixSum = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            prefixSum[i] = prefixSum[i - 1] + nums[i - 1];
        }
        for (int i = 1; i <= n; i++) {
            Arrays.fill(res[i], Integer.MAX_VALUE);
        }
        for (int i = 1; i <= n; i++) {
            int maxSplit = Math.min(i, k);
            for (int j = 1; j <= maxSplit; j++) {
                int resIJ = Integer.MAX_VALUE;
                for (int c = j - 1; c < i; c++) {
                    resIJ = Math.min(resIJ, Math.max(res[c][j - 1], prefixSum[i] - prefixSum[c]));
                }
                res[i][j] = resIJ;
            }
        }
        return res[n][k];
    }
}
```

## Binary Search Solution

没想到吧，这题还可以用二分法。不过如果你是刚开始刷题，或者对二分法理解还不算太透彻，不建议现在就看这个方法。等你刷完一圈题目，建立了基本的题感，然后专精一些二分法题目之后，再来看这个解法比较合适。

我们换个思路理解一下这个问题，给定一个数组，计算它最少可以分成多少个子数组，使得每个子数组的和都不大于 X。

举个例子，如果该数组最少可以分成 5 个子数组，使得每个子数组的和都不大于 8。那么该对于任意的 `X >= 8`，我们都可以说这个数组最少可以分成 5 个子数组使得每个子数组的和都不大于 X。反之，对于任意的 `X < 8`，我们都可以说我们没有办法把数组分成 4 个或者更少的子数组，使得每个子数组的和都不大于 X。

此时，题目就变成了给定一个 X，看看这个数组最少能分成多少个子数组使得每个子数组的和都不大于 X。
+ 假如可以分成 k 个，或者更少个，说明什么？说明我们可以分成 k 个子数组，并且每个子数组的和都不大于 X。那么我们就可以对 X 以及比 X 小的数里面继续找。
+ 假如可以分成 k + 1 个，或者更多个，说明什么？说明我们不可能分成 k 个子数组，使得每个之和都不大于 X。那么我们就得去比 X 大的数里面去寻找。

我们的目标就是在满足可以分成 k 个子数组的条件下，让 X 尽可能的小。

既然是二分法了，初始的左右端点怎么说？初始化左端点是整个数组最大的数。为什么？你想想看，如果数组有 10 个数，我给分成 10 个子数组。那么我只能保证每个子数组的和不大于最大的那个数，对不对？初始化右端点是整个数组的和。为什么？如果k 是 1 呢……对吧。

Time complexity: O(N * log(array sum))

Space complexity: O(1)

```java
class Solution {
    public int splitArray(int[] nums, int k) {
        int maxNum = nums[0];
        int sum = 0;
        for (int num : nums) {
            maxNum = Math.max(maxNum, num);
            sum += num;
        }
        int l = maxNum;
        int r = sum;
        while (l < r) {
            int mid = l + (r - l) / 2;
            int count = minSplitRequired(nums, mid);
            if (count <= k) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

    private int minSplitRequired(int[] nums, int maxSum) {
        int count = 1;
        int currSum = 0;
        for (int num : nums) {
            if (currSum + num <= maxSum) {
                currSum += num;
            } else {
                currSum = num;
                count++;
            }
        }
        return count;
    }
}
```