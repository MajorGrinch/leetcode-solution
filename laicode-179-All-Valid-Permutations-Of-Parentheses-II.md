# Laicode 179. All Valid Permutations Of Parentheses II

给定三种括号，以及各自的对数，枚举所有的组合可能。

这题和[22. Generate Parentheses](22-Generate-Parentheses.md)还是一个思路，只不过括号种类多了。所以我们需要一个数组来盛放三种括号的各自左右括号的剩余个数，三种括号对应的左右括号，以及用一个Stack来记录未被配对的左括号。

枚举三类括号的左右括号，当是左括号时，我们就去检查对应的这个左括号是否还有余额，有的话就加入当前的组合，余额减一，栈顶放入这个左括号然后进入下一层遍历。退出来的时候，记得恢复状态。当是右括号时，先检查当前的右括号是否有余额以及栈顶的左括号是否可以与其配对。如果都满足的话，就加入当前组合，余额减一，栈顶弹出，向下搜索。搜索出来之后记得恢复状态。

Time complexity: O(x * x!) where x = l + m + n.

Space complexity: O(x) if the result doesn't count.

```java
public class Solution {
  public List<String> validParentheses(int l, int m, int n) {
    List<String> res = new ArrayList<>();
    int targetLen = (l + m + n) * 2;
    dfs(
        new int[] {l, l, m, m, n, n},
        new char[] {'(', ')', '<', '>', '{', '}'},
        new ArrayDeque<>(),
        targetLen,
        new StringBuilder(),
        res);
    return res;
  }

  private void dfs(
      int[] lmnRemain,
      char[] lmnChars,
      Deque<Character> stack,
      int targetLen,
      StringBuilder permutation,
      List<String> res) {
    if (permutation.length() == targetLen) {
      res.add(permutation.toString());
      return;
    }
    for (int i = 0; i < lmnRemain.length; i++) {
      if (i % 2 == 0) {
        if (lmnRemain[i] == 0) continue;
        permutation.append(lmnChars[i]);
        lmnRemain[i]--;
        stack.push(lmnChars[i]);
        dfs(lmnRemain, lmnChars, stack, targetLen, permutation, res);
        stack.pop();
        lmnRemain[i]++;
        permutation.deleteCharAt(permutation.length() - 1);
      } else {
        if (stack.isEmpty() || lmnRemain[i] == 0 || stack.peek() != lmnChars[i - 1]) continue;
        permutation.append(lmnChars[i]);
        lmnRemain[i]--;
        stack.pop();
        dfs(lmnRemain, lmnChars, stack, targetLen, permutation, res);
        stack.push(lmnChars[i - 1]);
        lmnRemain[i]++;
        permutation.deleteCharAt(permutation.length() - 1);
      }
    }
  }
}
```