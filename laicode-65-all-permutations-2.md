# Laicode 65. All Permutations II

给定一个可能有重复字母的字符串，得到全排列。

参照[Leetcode 47](47-Permutations-II.md)

```java
public class Solution {
  public List<String> permutations(String input) {
    List<String> res = new ArrayList<>();
    if(input == null) {
      return res;
    }
    dfs(input.toCharArray(), 0, res);
    return res;
  }

  private void dfs(char[] sc, int index, List<String> res) {
    if(index == sc.length) {
      res.add(new String(sc));
      return;
    }
    Set<Character> used = new HashSet<>();
    for(int i = index; i < sc.length; i++) {
      if(!used.contains(sc[i])) {
        used.add(sc[i]);
        swap(sc, index, i);
        dfs(sc, index + 1, res);
        swap(sc, index, i);
      }
    }
  }

  private void swap(char[] sc, int i, int j){
    char temp = sc[i];
    sc[i] = sc[j];
    sc[j] = temp;
  }
}
```