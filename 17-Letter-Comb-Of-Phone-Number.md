# 17 Letter Combination of a Phone Number

给定一个电话表盘和一个`2-9`的按键序列，生成所有可能的字母组合。

经典的DFS题目，我们用一个String数组盛放表盘的字母构成。DFS的时候，通过当前数字获取对应表盘上的所有字母进行遍历，直到最后一个数字。base case就是遍历完所有的数字了，我们把当前的组合加入答案。

Time complexity: O(N * 4^N). 4^N combination and N to copy the comb to result list.

Space complexity: O(N). N level call stacks.

```java
class Solution {
  public List<String> letterCombinations(String digits) {
    List<String> res = new ArrayList<>();
    if (digits.length() == 0) {
      return res;
    }
    String[] pad = new String[] {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    dfs(digits.toCharArray(), 0, pad, new StringBuilder(), res);
    return res;
  }

  private void dfs(char[] digits, int level, String[] pad, StringBuilder comb, List<String> res) {
    if (level == digits.length) {
      res.add(comb.toString());
      return;
    }
    int number = digits[level] - '0';
    String candidates = pad[number];
    for (int i = 0; i < candidates.length(); i++) {
      comb.append(candidates.charAt(i));
      dfs(digits, level + 1, pad, comb, res);
      comb.deleteCharAt(comb.length() - 1);
    }
  }
}
```