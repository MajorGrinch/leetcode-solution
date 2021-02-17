# 22. Generate Parentheses

给一个数字，生成所有合理的括号对。

还是用DFS的思路，搜索过程中记录下现有的左括号，右括号数量，当目前左括号的数量还比n小的时候，就可以安插左括号。当前左括号数量比右括号多的时候，就可以安插右括号。当左右括号之和等于2n的时候，一个完整的permutation就生成了，加入答案。

Time complexity: O(n*2^(2n))，因为最后会有2^2n量级的答案。注意是2^2n量级，而不是2^2n个答案，因为不是每个位置都可以放右括号。每加入一个排列，`toString`方法会有n的复杂度，相乘就是这里的复杂度。

Space complexity: O(n)，2n层调用栈，2n长度的StringBuilder来记录排列。

```java
class Solution {
  public List<String> generateParenthesis(int n) {
    List<String> res = new ArrayList<>();
    if (n < 0) {
      return res;
    }
    dfs(n, 0, 0, new StringBuilder(), res);
    return res;
  }

  private void dfs(int n, int left, int right, StringBuilder permutation, List<String> res) {
    if (left + right == n * 2) {
      res.add(permutation.toString());
      return;
    }
    if (left < n) {
      permutation.append('(');
      dfs(n, left + 1, right, permutation, res);
      permutation.deleteCharAt(permutation.length() - 1);
    }
    if (left > right) {
      permutation.append(')');
      dfs(n, left, right + 1, permutation, res);
      permutation.deleteCharAt(permutation.length() - 1);
    }
  }
}
```