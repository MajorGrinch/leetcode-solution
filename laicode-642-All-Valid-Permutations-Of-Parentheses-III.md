# Laicode 642. All Valid Permutations Of Parentheses III

这题和[laicode 179. All Valid Permutations Of Parentheses II](laicode-179-All-Valid-Permutations-Of-Parentheses-II.md)相似，但是这题括号有优先级。只能优先级高的包进优先级低的，优先级高的不能被优先级低的给包括进去，同级也不能互相包括。

针对这个限制，我们压入栈就不能像179题那样压入字符了，而是压入对应的优先级数字。这样我们在压入左括号的时候就不仅检查这个左括号是否还有余额，如果栈为空或者栈顶括号对应的优先级大于当前的优先级，那么当前的左括号才可以压入。压入右括号的时候，检查的情况不变，栈不为空而且栈顶得是对应的左括号方可将此右括号加入搜索。

Time complexity: O(x * x!) where x = l + m + n.

Space complexity: O(x) if the result doesn't count.

```java
public class Solution {
  public List<String> validParenthesesIII(int l, int m, int n) {
    List<String> res = new ArrayList<>();
    int targetLen = (l + m + n) * 2;
    int[] lmnRemain = new int[] {l, l, m, m, n, n};
    char[] lmnChars = new char[] {'(', ')', '<', '>', '{', '}'};
    dfs(lmnRemain, lmnChars, targetLen, new ArrayDeque<>(), new StringBuilder(), res);
    return res;
  }

  private void dfs(
      int[] lmnRemain,
      char[] lmnChars,
      int targetLen,
      Deque<Integer> stack,
      StringBuilder permutation,
      List<String> res) {
    if (permutation.length() == targetLen) {
      res.add(permutation.toString());
      return;
    }
    for (int i = 0; i < lmnRemain.length; i++) {
      if (i % 2 == 0) {
        // left paretheses
        if (lmnRemain[i] > 0 && (stack.isEmpty() || stack.peek() > i)) {
          stack.push(i);
          permutation.append(lmnChars[i]);
          lmnRemain[i]--;
          dfs(lmnRemain, lmnChars, targetLen, stack, permutation, res);
          lmnRemain[i]++;
          permutation.deleteCharAt(permutation.length() - 1);
          stack.pop();
        }
      } else {
        // right parentheses
        if (!stack.isEmpty() && stack.peek() == i - 1) {
          stack.pop();
          permutation.append(lmnChars[i]);
          lmnRemain[i]--;
          dfs(lmnRemain, lmnChars, targetLen, stack, permutation, res);
          lmnRemain[i]++;
          permutation.deleteCharAt(permutation.length() - 1);
          stack.push(i - 1);
        }
      }
    }
  }
}
```