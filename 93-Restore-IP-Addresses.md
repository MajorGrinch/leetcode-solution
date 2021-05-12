# 93. Restore IP Addresses

给定一个数字序列，要求生成所有可能的ip地址。简单地说，IPV4可以用四个十进制数来表示，所以我们要把整个输入切成四段，每一段自成一个十进制数。

这题还是DFS。我们还是一位一位地来遍历题目的输入，同时用一个`List<Integer>`来记录目前为止生成的数字。base case就是目前生成了4个数字，如果`level == sc.length`就代表我们正好遍历完了整个输入，那么当前的四个数字变成ip地址字符串放入答案。如果没遍历完，就不能加入答案。但是无论是否遍历完都需要返回，因为ipv4只能有4个十进制数。

如果目前还没生成四个数字，那么当前段的十进制数就从level开始。我们就从level开始向后逐位遍历得到目前段的数字然后向下一段搜索，直到当前段的数字超过255为止。不过需要注意的是，如果level这一位本身就是0的话，那么这一段的十进制数只有可能是0，因为题目不允许leading zero。

Time complexity: O(27). Each number can only have up to 3 digits. 3 * 3 * 3 = 27.

Space complexity: O(1). Constant space because we have have 4 level call stacks.

```java
class Solution {
  public List<String> restoreIpAddresses(String s) {
    List<String> res = new ArrayList<>();
    dfs(s.toCharArray(), 0, new ArrayList<>(), res);
    return res;
  }

  private void dfs(char[] sc, int level, List<Integer> comb, List<String> res) {
    if (comb.size() == 4) {
      if (level == sc.length) {
        res.add(combToIP(comb));
      }
      return;
    }
    int currNum = 0;
    for (int i = level; i < sc.length; i++) {
      currNum = currNum * 10 + sc[i] - '0';
      if (currNum > 255) break;
      comb.add(currNum);
      dfs(sc, i + 1, comb, res);
      comb.remove(comb.size() - 1);

      /**
       * currNum == 0 indicates sc[level] == '0'. IP number can't have leading zero except 0 itself.
       */
      if (currNum == 0) break;
    }
  }

  private String combToIP(List<Integer> comb) {
    StringBuilder ipBuilder = new StringBuilder();
    for (int num : comb) {
      ipBuilder.append(num).append('.');
    }
    ipBuilder.deleteCharAt(ipBuilder.length() - 1);
    return ipBuilder.toString();
  }
}
```