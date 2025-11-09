# 55. Jump Game

给定一个数组的数字，每个下标的数字代表这个下标的地方能往后走几步。问从0开始，是否能走到最后一个下标。

## DP Approach 1

这题运用动态规划的思路，用 reachable 数组来存放每个下标是否可达。初始化第一个下标可达，其余都不可达。

从 0 开始遍历，如果当前`i`不可达，那么直接返回 false 就好了，因为末尾肯定没法可达。如果当前`i`是可达的，那么`[i + 1, i + nums[i]]`也全都可达，注意`i + nums[i]`不能数组越界。

最后看看末尾的下标是否可达就好。

```java
class Solution {
    public boolean canJump(int[] nums) {
        int n = nums.length;
        boolean[] reachable = new boolean[n];
        reachable[0] = true;
        for (int i = 0; i < n; i++) {
            if (!reachable[i]) {
                return false;
            }
            for (int k = 1; k <= nums[i] && i + k < n; k++) {
                reachable[i + k] = true;
            }
        }
        return reachable[n - 1];
    }
}
```

## DP Approach 2

这题还可以从末尾向前遍历，reachable 存的就是当前下标是否可以到达末尾。

那么`reachEnd[len - 1]`肯定是可以到达末尾的，它自己就是末尾。我们从倒数第二个下标开始往前推，每到一个下标为`i`的地方，就看看`[i + 1, i + nums[i]]`这个范围里有没有能走到最后的下标，如果有的话就代表`i`可以走到最后。如果没有，就说明`i`不可以走到最后。就这样一直到最开始的下标，我们就可以知道它到底可不可以走到最后。

Time complexity: O(N^2)

Space complexity: O(N)

```java
class Solution {
    public boolean canJump(int[] nums) {
        int n = nums.length;
        boolean[] reachable = new boolean[n];
        reachable[n - 1] = true;
        for (int i = n - 2; i >= 0; i--) {
            for (int j = i + 1; j <= i + nums[i] && j < n; j++) {
                if (reachable[j]) {
                    reachable[i] = true;
                    break;
                }
            }
        }
        return reachable[0];
    }
}
```

## Greedy Approach

这题也可以用贪心思想来解决，不过需要注意的是不是所有的动态规划都可以用贪心思想来做。满足贪心思想方法的动态规划需要满足几个条件，在这里不赘述，算法导论里有讲。

我们用`currReachable`代表目前走过的下标最远可以到达哪个下标。那么既然是从0出发，那么一开始currReachable自然初始化为0。如果当前`i`已经超过了`currReachable`说明之前所有的下标都没有办法走到这里，也就自然无法到达最后。否则，我们时刻更新currReachable，取它自己和`i + nums[i]`里的大的那一个。

只要循环能顺利结束，就说明可以走到最后。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
  public boolean canJump(int[] nums) {
    int currReachable = 0;
    for (int i = 0; i < nums.length; i++) {
      if (i > currReachable) {
        return false;
      }
      currReachable = Math.max(currReachable, i + nums[i]);
    }
    return true;
  }
}
```