# Laicode 176. Longest Common Substring

给定两个字符串找出最长公共子串。

用`lcs[i][j]`来记录以公共子串以`source[i-1]`结尾和以`target[j-1]`结尾可以有多长，注意这个公共子串必须是以他们为结尾，也就是说如果存在这个公共子串的话，`source[i-1]`和`target[j-1]`是相等的。那么我们就有公式可以计算`lsc[i][j] = lcs[i-1][j-1] + 1 if source[i-1]==target[j-1], else 0`。

双重for循环遍历，最后就可以得到最长的公共子串的长度和末端位置，最后可以得到最长公共子串。

Time complexity: O(m * n)

Space complexity: O(m * n)

```java
public class Solution {
  public String longestCommon(String source, String target) {
    if (source.length() == 0 || target.length() == 0) {
      return "";
    }
    int m = source.length(), n = target.length();
    int[][] lcs = new int[m + 1][n + 1];
    int lcsLen = 0;
    int endIndex = -1;
    for (int i = 1; i <= m; i++) {
      for (int j = 1; j <= n; j++) {
        if (source.charAt(i - 1) == target.charAt(j - 1)) {
          lcs[i][j] = lcs[i - 1][j - 1] + 1;
        }
        if (lcs[i][j] > lcsLen) {
          lcsLen = lcs[i][j];
          endIndex = i - 1;
        }
      }
    }
    return lcsLen == 0 ? "" : source.substring(endIndex - lcsLen + 1, endIndex + 1);
  }
}
```

```java
public class Solution {
  public String longestCommon(String source, String target) {
    int m = source.length();
    int n = target.length();
    int[][] lcs = new int[m + 1][n + 1];
    int resLen = 0;
    int resIdx = -1;
    for(int i = 1; i <= m; i++) {
      for(int j = 1; j <= n; j++) {
        lcs[i][j] = source.charAt(i - 1) == target.charAt(j - 1) ? lcs[i - 1][j - 1] + 1 : 0;
        if(resLen < lcs[i][j]) {
          resLen = lcs[i][j];
          resIdx = i - 1;
        }
      }
    }
    return source.substring(resIdx - resLen + 1, resIdx + 1);
  }
}
```