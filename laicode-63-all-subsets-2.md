# Laicode 63. All Subsets II

给定一个含有重复字母的字符串，返回所有子串，答案不能含有重复子串。

其实就是经典的DFS求subset，和[leetcode 90](90-Subsets-II.md)一样。

我们需要先把字符串变成char数组，然后按照字符排序。DFS的时候我们需要跳过连续的重复字母以避免重复答案，index只要入了当前的候选答案，接下来所有和index相同的字母都得跳过。

Time complexity: O(2^n)

Space complexity: O(n), n level call stack

```java
public class Solution {
  public List<String> subSets(String set) {
    List<String> res = new ArrayList<>();
    if(set == null) {
      return res;
    }
    char[] sc = set.toCharArray();
    Arrays.sort(sc);
    dfs(sc, 0, new StringBuilder(), res);
    return res;
  }

  private void dfs(char[] sc, int index, StringBuilder sb, List<String> res) {
    if(index == sc.length) {
      res.add(sb.toString());
      return;
    }
    sb.append(sc[index]);
    dfs(sc, index + 1, sb, res);

    sb.deleteCharAt(sb.length() - 1);
    int i = index + 1;
    while(i < sc.length && sc[i] == sc[index]) {
      i++;
    }
    dfs(sc, i, sb, res);
  }
}
```