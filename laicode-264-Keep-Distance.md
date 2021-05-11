# Laicode 264. Keep Distance For Identical Elements

给定一个数字k，要求生成一个[1,1,2,3...k,k]组成的序列，该序列满足任何两个相同的数x之间有x个元素。如果存在该序列就返回序列，不存在就返回null。

还是经典的DFS题目，我们可以根据题目的要求来搜索。我们可以从k开始往前搜，因为这样可以一开始减少分支情况从而降低一些复杂度。

按照题目要求，相同元素x之间必须间隔x个元素，这样去不断地搜索。base case就是间隔为0，说明序列已经生成好了，就返回true表示有这么个序列。否则，我们就从下标0开始，遍历所有可以安排的位置，然后逐一向下搜索。只要下层返回true，就代表下层搜索找到了一个合理的序列，当前层也就立刻停止搜索向上返回true。

最后只要最上层的搜索结果是true，我们就可以返回这个序列了。否则，返回null代表没有结果。

Time complexity: O(k!)

Space complexity: O(k). k levels of call stack.

```java
public class Solution {
  public int[] keepDistance(int k) {
    int[] array = new int[k * 2];
    if (dfs(array, k)) {
      return array;
    } else {
      return null;
    }
  }

  private boolean dfs(int[] array, int dis) {
    if (dis == 0) {
      return true;
    }
    for (int i = 0; i < array.length - dis - 1; i++) {
      if (array[i] == 0 && array[i + dis + 1] == 0) {
        array[i] = dis;
        array[i + dis + 1] = dis;
        if (dfs(array, dis - 1)) {
          return true;
        }
        array[i] = 0;
        array[i + dis + 1] = 0;
      }
    }
    return false;
  }
}
```