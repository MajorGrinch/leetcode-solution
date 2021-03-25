# 55. Jump Game

给定一个数组的数字，每个下标的数字代表这个下标的地方能往后走几步。问从0开始，是否能走到最后一个下标。

## DP Approach

这题可以用动态规划的思想来解决，用`reachEnd`数组来存放子问题答案。`reachEnd[i]`表示`i`是否可以走到最后一个下标，`true`就是可以，`false`就是不可以。

`reachEnd[len - 1]`肯定是true，它自己就是最后一个下标。我们从倒数第二个下标开始往前推，每到一个下标为`i`的地方，就看看`[i, i + nums[i]]`这个范围里有没有能走到最后的下标，如果有的话就代表`i`可以走到最后。如果没有，就说明`i`不可以走到最后。就这样一直到最开始的下标，我们就可以知道它到底可不可以走到最后。

Time complexity: O(N^2)

Space complexity: O(N)

```java
class Solution {
  public boolean canJump(int[] nums) {
    boolean[] reachEnd = new boolean[nums.length];
    reachEnd[nums.length - 1] = true;
    for (int i = nums.length - 2; i >= 0; i--) {
      for (int j = i; j <= i + nums[i] && j < nums.length; j++) {
        if (reachEnd[j]) {
          reachEnd[i] = true;
        }
      }
    }
    return reachEnd[0];
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