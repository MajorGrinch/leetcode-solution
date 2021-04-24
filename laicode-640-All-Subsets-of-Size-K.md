# Laicode 640. All Subsets of Size K

这题和普通的求子集略有区别，这题子集最多有k个元素。

那么其实也不难，base caes稍有变化而已。这里base case就是当前子集元素个数达到k的时候直接停止向后遍历并加入答案。另一个base case就是遍历到最后了，但是只返回并不加入答案，因为如果满足的话之前就会加入答案并返回。

Time complexity: O(2^n)

Space complexity: O(n) if the result doesn't count.

```java
public class Solution {
  public List<String> subSetsOfSizeK(String set, int k) {
    List<String> res = new ArrayList<>();
    if (set == null) {
      return res;
    }
    subsetsK(set.toCharArray(), 0, k, new StringBuilder(), res);
    return res;
  }

  private void subsetsK(char[] sc, int level, int k, StringBuilder subset, List<String> res) {
    if (subset.length() == k) {
      res.add(subset.toString());
      return;
    }
    if (level == sc.length) {
      return;
    }

    subset.append(sc[level]);
    subsetsK(sc, level + 1, k, subset, res);
    subset.deleteCharAt(subset.length() - 1);

    subsetsK(sc, level + 1, k, subset, res);
  }
}
```