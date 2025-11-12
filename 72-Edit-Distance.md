# 72. Edit Distance

给定两个字符串，求出最少需要多少次更改就可以使得他们两个相同。更改包括增、删、改。

## Recursive Approach

此题直观上来说可以用递归的思路来解决。

base case是，任一字符串长度为0，那么需要更改的数量就是另一个字符串的长度。如果两个字符串的首个字符相同，那么更改距离其实就是除去首字符之后子串的更改距离。如果两个字符首个字符不相同，那么我们可以通过替换，删除或者增加来继续递归，直到遇到base case为止。

这样我们最后就可以计算出最少需要改动多少次。但是这个方法最致命的缺点就是大量的重复计算，比如这里的`word1[1, len1)`和`word2[2, len2)`至少会被重复计算三次。分别是：
+ word1[0, len1), word2[1, len2), replace
+ word1[0, len1), word2[2, len2), delete
+ word1[1, len1), word2[1, len2), insert

所以此方法会超时。

```java
class Solution {
  public int minDistance(String word1, String word2) {
    if (word1.length() == 0) {
      return word2.length();
    }
    if (word2.length() == 0) {
      return word1.length();
    }

    if (word1.charAt(0) == word2.charAt(0)) {
      return minDistance(word1.substring(1), word2.substring(1));
    }
    int replace = 1 + minDistance(word1.substring(1), word2.substring(1));

    int delete = 1 + minDistance(word1.substring(1), word2);

    int insert = 1 + minDistance(word1, word2.substring(1));

    return Math.min(replace, Math.min(delete, insert));
  }
}
```

## DP Approach

正是因为有这些大量的重复计算，导致普通递归解法会超时。有没有什么办法能够避免重复计算呢？那就是用动态规划来沿途记录子问题的答案，这样可以供上层直接使用。

`dis[][]`数组记录着子问题答案，`dis[i][j]`代表word1[0, i)到word2[0, j)需要多少步，那么重复的计算就会被节省出来。

使用双层for循环，从头开始遍历。`i == 0 && j == 0`，两个空串更改距离肯定是0。`i == 0 && j != 0`，那么更改距离就是`j`。`i != 0 && j == 0`，那么更改距离就是`i`。如果`i != 0 && j != 0`，那么说明两个都不是空串，那么word1[0, i)到word2[0, j)的更改距离有以下三种来源
+ word1[0, i)删去一个字符，然后由word1[0, i - 1)变到word2[0, j)，那么其实就是word1[0, i - 1)到word2[0, j)的距离加1。
+ word1[0, i)加上一个字符，再变到word2[0, j)，那么其实就相当于从word1[0, i)变成word2[0, j - 1)的距离加1。
+ word1[0, i)改变一个字符，再变到word2[0, j)，那么其实就相当于word[0, i - 1)变成word2[0, j - 1)的距离加1。但是呢，如果word1[i - 1] == word2[j - 1]的话，那么其实这里就不用多一步了。

以上三个来源，取最小的那个，就是word1[0, i)到word2[0, j)的更改距离。一直到最后我们就可以计算出`dis[word1.length()][word2.length()]`，也就是word1到word2的最小更改距离。

Time complexity: O(M * N), where M is the length of word1 and N is the length of word2.

Complexity: O(M * N)

```java
class Solution {
  public int minDistance(String word1, String word2) {
    if (word1.length() == 0) {
      return word2.length();
    }
    if (word2.length() == 0) {
      return word1.length();
    }
    int[][] dis = new int[word1.length() + 1][word2.length() + 1];
    for (int i = 0; i <= word1.length(); i++) {
      for (int j = 0; j <= word2.length(); j++) {
        if (i == 0 && j == 0) {
          dis[i][j] = 0;
        } else if (i == 0) {
          dis[i][j] = j;
        } else if (j == 0) {
          dis[i][j] = i;
        } else {
          int delDis = dis[i - 1][j] + 1;
          int addDis = dis[i][j - 1] + 1;
          int repDis = dis[i - 1][j - 1] + 1;
          if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
            repDis--;
          }
          dis[i][j] = Math.min(repDis, Math.min(addDis, delDis));
        }
      }
    }
    return dis[word1.length()][word2.length()];
  }
}
```

2025 code:

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int M = word1.length();
        int N = word2.length();
        int[][] distance = new int[M + 1][N + 1];
        for (int i = 0; i < M + 1; i++) {
            distance[i][0] = i;
        }
        for (int j = 0; j < N + 1; j++) {
            distance[0][j] = j;
        }
        for (int i = 1; i < M + 1; i++) {
            for (int j = 1; j < N + 1; j++) {
                int replace = word1.charAt(i - 1) == word2.charAt(j - 1) ? distance[i - 1][j - 1]
                        : distance[i - 1][j - 1] + 1;
                int delete = distance[i - 1][j] + 1;
                int insert = distance[i][j - 1] + 1;
                distance[i][j] = Math.min(replace, Math.min(delete, insert));
            }
        }
        return distance[M][N];
    }
}
```