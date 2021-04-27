# Laicode 643. All Permutations of Subsets

给定一个字符串，打印出所有的排列的子集且不能重复。比如`abc`的子集有个`bc`，`bca`的子集也有一个`bc`，但是答案里只能有一个`bc`。

其实和all permutation差不多，我们依然按照求全排列的方法去做，只是我们沿途记录下每一层递归时候已经排列好的部分就可以。因为已经排列好的部分肯定是后续所有衍生的全排列所共有的子集，所以记录下这个就可以保证子集不会重复。

Time complexity: O(N * N!)

Space complexity: O(N)

```java
public class Solution {
  public List<String> allPermutationsOfSubsets(String set) {
    List<String> res = new ArrayList<>();
    dfs(set.toCharArray(), 0, res);
    return res;
  }

  private void dfs(char[] sc, int index, List<String> res) {
    res.add(new String(sc, 0, index));
    if(index == sc.length) return;

    for (int i = index; i < sc.length; i++) {
      swap(sc, index, i);
      dfs(sc, index + 1, res);
      swap(sc, index, i);
    }
  }

  private void swap(char[] sc, int i, int j) {
    char temp = sc[i];
    sc[i] = sc[j];
    sc[j] = temp;
  }
}
```