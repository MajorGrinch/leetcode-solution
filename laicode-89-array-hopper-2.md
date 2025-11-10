# Laicode 89. Array Hopper II

## DP Approahc 1

参照[leetcode 45](45-Jump-Game-II.md)。

```java
public class Solution {
  public int minJump(int[] array) {
    int len = array.length;
    int[] steps = new int[len];
    Arrays.fill(steps, -1);
    steps[0] = 0;
    for(int i = 0; i < len; i++) {
      if(steps[i] == -1) {
        return -1;
      }
      for(int j = i + 1; j < len && j <= i + array[i]; j++) {
        if(steps[j] == -1) {
          steps[j] = steps[i] + 1;
        } else {
          steps[j] = Math.min(steps[j], steps[i] + 1);
        }
      }
    }
    return steps[len - 1];
  }
}
```


## Greedy Approach

参照[leetcode 45](45-Jump-Game-II.md)，但是这题有一点不一样就是不保证末尾可达，那么我们就需要做一些额外的修改。i 需要遍历每一个下标，当 currentReach 大于等于末尾下标的时候，返回已走的步数作为答案。否则，我们就正常更新 nextReach，然后当 i 触及到 currentReach 的时候，再向后走就需要 step++了。

如果出了循环，说明末尾不可达，直接返回-1。为什么出了循环说明末尾不可达？想一下，循环结束了，我们都没找到 currentReach >= array.length - 1 的情况，说明末尾不可达。如果还是不能理解的话，就想象一下，在循环里出现 i > currentReach，是什么情况？说明可达范围已经没法延伸了，后面所有的 i 其实都没有意义，因为后面所有的 i 都不可达。

```java
public class Solution {
  public int minJump(int[] array) {
    int steps = 0;
    int currentReach = 0;
    int nextReach = array[0];
    for(int i = 0; i < array.length; i++) {
      if(currentReach >= array.length - 1) {
        return steps;
      }
      nextReach = Math.max(nextReach, i + array[i]);
      if(i == currentReach) {
        steps++;
        currentReach = nextReach;
      }
    }
    return -1;
  }
}
```