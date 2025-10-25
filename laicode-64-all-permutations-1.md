# Laicode 64. All Permutations I

给定一个无重复字符的字符串，找出它所有的可能permutation。

经典DFS，参照 [Leetcode 46](46-Permutation.md)

```java
public class Solution {
  public List<String> permutations(String input) {
    List<String> res = new ArrayList<>();
    dfs(input.toCharArray(), 0, res);
    return res;
  }

  private void dfs(char[] sc, int index, List<String> res) {
    if(index == sc.length) {
      res.add(new String(sc));
      return;
    }
    for(int i = index; i < sc.length; i++) {
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