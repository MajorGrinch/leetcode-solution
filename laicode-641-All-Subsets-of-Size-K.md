# Laicode 641. All Subsets II of Size K

给定一个字符串，求出所有长度为k的子集。

这题结合[laicode 640. All Subsets of Size K](laicode-640-All-Subsets-of-Size-K.md)和[90. Subsets II](90-Subsets-II.md)两个思路，前者用来借鉴求k个元素的子集，后者来借鉴处理重复元素的情况。

Time complexity: O(k * C_N_k + 2^N). N is the length of the string. We have to iterate all subsets and find the subset with k elements to add them into the result list.

Space complexity: O(N). N level call stack.

```java
public class Solution {
  public List<String> subSetsIIOfSizeK(String set, int k) {
    List<String> res = new ArrayList<>();
    char[] sc = set.toCharArray();
    Arrays.sort(sc);
    dfs(sc, k, 0, new StringBuilder(), res);
    return res;
  }

  private void dfs(char[] sc, int k, int index, StringBuilder subset, List<String> res) {
    if (index == sc.length) {
      if (subset.length() == k) res.add(subset.toString());
      return;
    }

    char curr = sc[index];
    subset.append(curr);
    dfs(sc, k, index + 1, subset, res);
    subset.deleteCharAt(subset.length() - 1);

    while (index < sc.length && sc[index] == curr) index++;
    dfs(sc, k, index, subset, res);
  }
}
```