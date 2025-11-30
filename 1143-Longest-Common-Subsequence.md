# 1143. Longest Common Subsequence

给定两个字符串，找出最长公共序列。不同于最长公共子串，序列是可以由不连续的字符组成。

这题的话也是经典的动态规划题，动态规划就是把大问题切割成子问题并且从子问题的答案中得到自身的答案。那么具体到这题，我们该怎么切割？`lcs[i][j]`是`text1[0, i)`和`text2[0, j)`能组成的最长公共序列，切割这个问题要分两种情况。

第一种情况，`text[i - 1] != text2[j - 1]`，即结尾字符不同，那么`lcs[i][j] = MAX(lcs[i - 1][j], lcs[i][j - 1])`。这个不难理解，结尾字符不同，那么就各自扔掉结尾的字符再去看看剩余的部分和对方能产生的最长公共序列。

第一种情况，`text1[i - 1] == text2[j - 1]`，即结尾字符相同，那么`lcs[i][j] = lcs[i - 1][j - 1] + 1`。这个也不难理解，结尾字符相同那么就同时扔掉结尾字符得到剩余部分的最长公共序列长度再加一就好了。但是有个问题，我们怎么知道一定就是来自于这个子问题，而不是来自于第一种情况的两个子问题，即`MAX(lcs[i - 1][j], lcs[i][j - 1])`。其实我们可以想一想，`lcs[i-1][j]`是不可能大于`lcs[i-1][j-1] + 1`的。为什么？假如`lcs[i-1][j] > lcs[i-1][j-1] + 1`的话，我们就可以推导出`lcs[i-1][j-1] >= lcs[i-1][j-1] + 1`，这显然不可能。所以，我们可以认为子问题答案是来自于`lcs[i-1][j-1] + 1`。

那么最后`lcs[m][n]`也就是两个字符串的最长公共序列的长度。

Time complexity: O(m * n)

Space complexity: O(m * n)

```java
class Solution {
  public int longestCommonSubsequence(String text1, String text2) {
    if (text1.length() == 0 || text2.length() == 0) {
      return 0;
    }

    int m = text1.length(), n = text2.length();
    int[][] lcs = new int[m + 1][n + 1];
    int lcsLen = 0;
    for (int i = 1; i <= m; i++) {
      for (int j = 1; j <= n; j++) {
        if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
          lcs[i][j] = lcs[i - 1][j - 1] + 1;
        } else {
          lcs[i][j] = Math.max(lcs[i - 1][j], lcs[i][j - 1]);
        }
      }
    }
    return lcs[m][n];
  }
}
```

## Optimized Space

通过刚刚的方法，我们发现当前问题的答案要么来自于同一行，要么来自于上一行，所以我们可以用两个一维数组，一个放当前行，一个放前一行。这样可以把空间复杂度压到O(min(m, n))。

```java
class Solution {
  public int longestCommonSubsequence(String text1, String text2) {
    if (text1.length() == 0 || text2.length() == 0) {
      return 0;
    }

    int m = text1.length(), n = text2.length();
    if (m > n) {
      return longestCommonSubsequence(text2, text1);
    }
    int[] prev = new int[n + 1];
    int lcsLen = 0;
    for (int i = 1; i <= m; i++) {
      int[] lcs = new int[n + 1];
      for (int j = 1; j <= n; j++) {
        if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
          lcs[j] = prev[j - 1] + 1;
        } else {
          lcs[j] = Math.max(prev[j], lcs[j - 1]);
        }
      }
      prev = lcs;
    }
    return prev[n];
  }
}
```

2025 code 这次真正做到优化空间，我觉得 2021 年那次并不是真正的优化空间。

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length();
        int n = text2.length();
        int[] prevLCS = new int[n + 1];
        int[] currLCS = new int[n + 1];
        for (int i = 1; i <= m; i++) {
            Arrays.fill(currLCS, 0);
            for (int j = 1; j <= n; j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    currLCS[j] = prevLCS[j - 1] + 1;
                } else {
                    currLCS[j] = Math.max(prevLCS[j], currLCS[j - 1]);
                }
            }
            int[] temp = prevLCS;
            prevLCS = currLCS;
            currLCS = temp;
        }
        return prevLCS[n];
    }
}
```