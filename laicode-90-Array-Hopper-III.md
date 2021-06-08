# Laicode 90. Array Hopper III

和[45. Jump Game II](45-Jump-Game-II.md)，但是这里是求出跳出数组所需的步数，而不是调到最后一个下标。那其实也还是一样的。

## Dynamic Programming

`steps[i]`存放的是`i`跳出数组需要多少步。我们把`steps`初始化为`n + 1`，因为是要跳出数组。`steps[n] = 0`，然后从`n-1`向前遍历，求出每个`steps[i]`，最后就知道是否可达以及要多少步了。

Time complexity: O(n^2)

Space complexity: O(n)

```java
public class Solution {
  public int minJump(int[] array) {
    int n = array.length;
    int[] steps = new int[n + 1];
    steps[n] = 0;
    for (int i = n - 1; i >= 0; i--) {
      int localMin = Integer.MAX_VALUE;
      for (int j = i + 1; j <= i + array[i] && j <= n; j++) {
        localMin = Math.min(localMin, steps[j]);
      }
      steps[i] = localMin == Integer.MAX_VALUE ? localMin : localMin + 1;
    }
    return steps[0] == Integer.MAX_VALUE ? -1 : steps[0];
  }
}
```

## Greedy Approach

同样的，这题也可以用贪心思想来解决。`steps`表示目前走了多少步，初始化为0。`currReachable`表示目前走的步数足够走到哪儿，`nextReachable`表示如果步数加一则足够走到哪儿。显然，当`steps = 0`的时候，`currReachable = 0`，`nextReachable = array[0]`。

遍历每个下标，首先更新`nextReachable`。接着判断，如果`i == currReachable`表示目前步数只够到这里，那么我们就得多走一步，`currReachable`也得变成多走一步能走到的位置即`nextReachable`。为什么要先更新`nextReachable`，因为当`i`正好走到`currReachable`的时候，当前`i`能到的地方自然也是当前`steps + 1`能走到的地方。

遍历完之后，我们看`currReachable`是否能够跳出数组，能的话就返回步数，不能就返回`-1`。

Time complexity: O(n)

Space complexity: O(1)

```java
public class Solution {
  public int minJump(int[] array) {
    int steps = 0;
    int currReachable = 0, nextReachable = array[0];
    for (int i = 0; i < array.length; i++) {
      nextReachable = Math.max(nextReachable, i + array[i]);
      if (i == currReachable) {
        steps++;
        currReachable = nextReachable;
      }
    }
    return currReachable >= array.length ? steps : -1;
  }
}
```