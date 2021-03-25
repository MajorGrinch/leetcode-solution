# 45. Jump Game II

## DP Approach

和 [55. Jump Game](55-Jump-Game.md) 差不多，但是这里需要求出最少走几步。

那么我们依然用动态规划的思想，`minJumps[i]`代表`i`最少需要几步才可以走到最后的位置，`minJumps[len - 1]`是最后一个下标所以显然为0。从倒数第二个位置往前推，每到下标`i`的地方就看看`[i + 1, i + nums[i]]`范围内谁到最后的步数最少，取最少的那个加上1就是`i`到最后需要的步数。

最后我们返回`minJumps[0]`就可以了。

Time complexity: O(N^2)

Space complexity: O(N)

```java
class Solution {
  public int jump(int[] nums) {
    int[] minJumps = new int[nums.length];
    minJumps[nums.length - 1] = 0;
    for (int i = nums.length - 2; i >= 0; i--) {
      int localMinJumps = Integer.MAX_VALUE;
      for (int j = i + 1; j <= i + nums[i] && j < nums.length; j++) {
        if (minJumps[j] != -1) {
          localMinJumps = Math.min(localMinJumps, minJumps[j]);
        }
      }
      minJumps[i] = localMinJumps == Integer.MAX_VALUE ? -1 : localMinJumps + 1;
    }
    return minJumps[0];
  }
}
```

## Greedy Approach

这题依然可以用贪心思想来实现。

`steps`表示目前已经走了多少步，`currReachable`表示目前已经走的步数足够到哪个下标，`nextReachable`表示目前已走的步数再多一步足够到哪儿。那么显然，steps初始化为0。走了0步肯定是哪儿都到不了所以`currReachable`也是0，表示目前已经走的步数足够到0。`nextReachable`表示再走一步，那么显然是足够到`nums[0]`下标的。

`i`从0开始，每次先更新目前已走步数再多走一步能到哪儿，然后看看`i`是否能够往后走但是不用加一个step。那其实也就是看`i`是否到`currReachable`了，如果到了的话`i`想要继续往后就不得不给step加一，并且把`currReachable`更新为`nextReachable`。如果还没到那么`i`继续走就不用给`step`加一，继续走就好了。

此外还有一个问题就是`i`走到什么时候停下来？走到`nums.length - 1`吗？并不是。因为题目保证一定可以走到最后，只是要计算出最少需要几步。所以直接走到倒数第二个下标就行了，这样就可以防止如果`currReachable`正好停在最后，导致step不必要的加一操作。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
  public int jump(int[] nums) {
    int steps = 0;
    int currReachable = 0, nextReachable = nums[0];
    for (int i = 0; i < nums.length - 1; i++) {
      nextReachable = Math.max(nextReachable, i + nums[i]);
      if (i == currReachable) {
        steps++;
        currReachable = nextReachable;
      }
    }
    return steps;
  }
}
```