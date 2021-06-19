# 131. Palindrome Partitioning

给定一个字符串，返回它所有的回文子串切割方法。

这题我们可以用DFS，然后配合检查给定子串是否是回文来进行搜索。深搜本来就已经挺耗时了，如果还要实时判定子串是否是回文就更加费时，所以我们其实可以提前计算好给定字符串所有的子串是否是回文。`isPalindrome[i][j]`表示`s[i, j]`是否是回文，那么显然它是个`n * n`的二维数组。

那么怎么计算`isPalindrome[][]`呢？首先`isPalindrome[i][i]`肯定都是`true`，单个字符当然是回文。接着，`isPalindrome[i][i + 1]`则取决于`s[i] == s[i + 1]`。这两种base case的子问题都好计算出结果以供general case子问题来调用。根据`isPalindrome[][]`的定义可得到状态转移方程，`isPalindrome[i][j] = isPalindrome[i + 1][j - 1] && s[i] == s[j]`，意思是`s[i + 1][j - 1]`是回文然后`s[i]`和`s[j]`还相等。由此我们可知`isPalindrome[i][j]`取决于`isPalindrome[i + 1][j - 1]`，也就是下一行的结果，所以我们从`n-3`行开始往前计算。

Time complexity: O(N^2 + N * 2^N) = O(N * 2^N) for worst scenario.

Space complexity: O(N * N)

```java
class Solution {
  public List<List<String>> partition(String s) {
    List<List<String>> res = new ArrayList<>();
    if (s == null || s.length() == 0) {
      return res;
    }

    boolean[][] isPalindrome = calculatePalindrome(s);
    partition(s, 0, new ArrayList<>(), res, isPalindrome);
    return res;
  }

  private void partition(
      String s, int start, List<String> comb, List<List<String>> res, boolean[][] isPalindrome) {
    if (start == s.length()) {
      res.add(new ArrayList<>(comb));
      return;
    }
    for (int end = start; end < s.length(); end++) {
      if (isPalindrome[start][end]) {
        comb.add(s.substring(start, end + 1));
        partition(s, end + 1, comb, res, isPalindrome);
        comb.remove(comb.size() - 1);
      }
    }
  }

  private boolean[][] calculatePalindrome(String s) {
    int n = s.length();
    boolean[][] isPalindrome = new boolean[n][n];
    for (int i = 0; i < n; i++) {
      isPalindrome[i][i] = true;
      if (i < n - 1) {
        isPalindrome[i][i + 1] = s.charAt(i) == s.charAt(i + 1);
      }
    }
    for (int i = n - 3; i >= 0; i--) {
      for (int j = i + 2; j < n; j++) {
        isPalindrome[i][j] = isPalindrome[i + 1][j - 1] && (s.charAt(i) == s.charAt(j));
      }
    }
    return isPalindrome;
  }
}
```