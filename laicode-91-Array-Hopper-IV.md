# Laicode 91. Array Hopper IV

和前面几题略有出入，这里我们会从指定的点出发，每个点都可以向左向右跳。求出最后跳到最后一个下标需要多少步。

那么这样的话，我们需要明确一点就是既然目标是最后一个下标，那么肯定是尽量往右跳。但是呢，说不准左边哪个点可以一下子跳很远，所以也不能忽略。因此这题我们的思路是，先求出起点到其左边所有点的最短步数。然后，我们再求出起点到其右边所有点的最短步数。

求起点到其左边点的最短步数时，不一定就是从起点出发一直往左跳，很有可能从起点跳到右边一个点再跳到左边反而步数更少。所以我们先求出起点只向右跳从而到达右边各点的最短步数，然后我们就可以求出起点到左边各点的最短步数。最后，我们再次求出起点到达右边各点的最短步数，只是这次不限于只向右跳。

最后`steps[n - 1]`就是`index`出发需要多少步才能到最后一个下标。

Time complexity: O(n^2)

Space complexity: O(n)

```java
public class Solution {
  public int minJump(int[] array, int index) {
    if (index >= array.length || (index < array.length - 1 && array[index] == 0)) {
      return -1;
    }
    int n = array.length;
    int[] steps = new int[n];
    steps[index] = 0;

    for (int i = index + 1; i < n; i++) {
      int minStepsFromIndex = Integer.MAX_VALUE;
      for (int j = index; j < i; j++) {
        if (steps[j] != Integer.MAX_VALUE && j + array[j] >= i) {
          minStepsFromIndex = Math.min(minStepsFromIndex, steps[j] + 1);
        }
      }
      steps[i] = minStepsFromIndex;
    }

    for (int i = index - 1; i >= 0; i--) {
      int minStepsFromIndex = Integer.MAX_VALUE;
      for (int j = i + 1; j < n; j++) {
        if (steps[j] != Integer.MAX_VALUE && j - array[j] <= i) {
          minStepsFromIndex = Math.min(minStepsFromIndex, steps[j] + 1);
        }
      }
      steps[i] = minStepsFromIndex;
    }

    for (int i = index + 1; i < n; i++) {
      int minStepsFromIndex = Integer.MAX_VALUE;
      for (int j = 0; j < i; j++) {
        if (steps[j] != Integer.MAX_VALUE && j + array[j] >= i) {
          minStepsFromIndex = Math.min(minStepsFromIndex, steps[j] + 1);
        }
      }
      steps[i] = minStepsFromIndex;
    }

    return steps[n - 1] == Integer.MAX_VALUE ? -1 : steps[n - 1];
  }
}
```