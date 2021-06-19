# 132. Palindrome Partitioning II

给定一个字符串，求出把它全分割为回文子串所需的最少切割次数。这题可以换个问法，可以说求分割为最少数量的回文子串，我们求出最少的个数再减一不就是最小的切割次数了。

`minPalindrome[i]`代表`s[0, i)`最少回文子串的个数，那么有状态转移方程`minPalindrome[i] = MIN(minPalindrome[j]) + 1, j < i and s[j, i) is palindrome`。那么我们还是老规矩，先求出每个字串是否是回文，得到一个`isPalindrome[][]`数组。

显然，`minPalindrome[0] = 0`。我们从`1`开始一直到`s.length()`，求出每个`minPalindrome[i]`即`s[0, i)`最少回文子串个数。最后`minPalindrome[s.length()] - 1`就是最少切割次数。

Time complexity: O(N^2)

Space complexity: O(N^2)

```java
class Solution {
  public int minCut(String s) {
    int n = s.length();
    int[] minPalindromes = new int[n + 1];
    boolean[][] isPalindrome = calculatePalindrome(s);
    minPalindromes[0] = 0;
    for (int i = 1; i <= n; i++) {
      int localMin = i;
      for (int j = 0; j < i; j++) {
        if (isPalindrome[j][i - 1]) {
          localMin = Math.min(localMin, minPalindromes[j] + 1);
        }
      }
      minPalindromes[i] = localMin;
    }
    return minPalindromes[n] - 1;
  }

  private boolean[][] calculatePalindrome(String s) {
    int n = s.length();
    boolean[][] isPalindrome = new boolean[n][n];
    for (int i = 0; i < n; i++) {
      isPalindrome[i][i] = true;
      if (i < n - 1) isPalindrome[i][i + 1] = s.charAt(i) == s.charAt(i + 1);
    }
    for (int i = n - 3; i >= 0; i--) {
      for (int j = i + 2; j < n; j++) {
        isPalindrome[i][j] = isPalindrome[i + 1][j - 1] && s.charAt(i) == s.charAt(j);
      }
    }
    return isPalindrome;
  }
}
```

## Yet Another Implementation

我们也可以不用提前算出`isPalindrome[][]`，直接跟着一起算就好。因为对于当前`i`的每个`isPalindrome[j][i - 1]`来说，我们需要的是`isPalindrome[j + 1][i - 2]`，它必定在`i - 1`的时候就被计算出来了。那么对于当前`i`的每个`isPalindrome[j][i - 1]`怎么实时计算呢？也就是怎么判断`s[j, i - 1]`是回文呢？首先条件是`s[j] == s[i - 1]`否则一切免谈。满足这个条件之后，如果子串长度小于等于3，那么必定是回文。如果大于3，那么就取决于`isPalindrome[j + 1][i - 2]`。这样，我们就可以实时计算出当前`i`对应的每个`isPalindrome[j][i - 1]`，然后计算`minPalindrome[i]`。

不过貌似也没有省时间，也没有省空间，只是代码行数少了，思路也绕了点。哈哈哈。

```java
class Solution {
  public int minCut(String s) {
    int n = s.length();
    int[] minPalindromes = new int[n + 1];
    boolean[][] isPalindrome = new boolean[n][n];
    minPalindromes[0] = 0;
    for (int i = 1; i <= n; i++) {
      int localMin = i;
      for (int j = 0; j < i; j++) {
        if (s.charAt(j) == s.charAt(i - 1)) {
          isPalindrome[j][i - 1] = (i - j <= 3) || isPalindrome[j + 1][i - 2];
        }
        if (isPalindrome[j][i - 1]) {
          localMin = Math.min(localMin, minPalindromes[j] + 1);
        }
      }
      minPalindromes[i] = localMin;
    }
    return minPalindromes[n] - 1;
  }
}
```