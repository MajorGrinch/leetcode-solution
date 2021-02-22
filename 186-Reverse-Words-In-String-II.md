# 186. Reverse Words in a String II

给定一个句子，把整个句子的单词反转过来。比如`the sky is blue`变成`blue is sky the`。

Clarification:
+ 有无leading/trailing zero？没有
+ 单词之间只有一个空格？是

问清楚具体的条件之后，思路很简单了。既然是单词对调，那么我们就先把整个字符串反转，然后再把每个单词自身再反转一下就好了。比如`the sky is blue`->`eulb si yks eht`->`blue is sky the`。

具体过程就是先把字符串反转，然后遍历。`wordStart`记录当前单词的开始位置，当`i + 1`是空格时说明当前是一个单词的结束，直接反转本单词。空格后肯定是下一个单词，所以再更新一下wordStart为空格的后一个位置就好。

不过需要注意的是由于没有leading/trailing zero，所以我们单独处理`i == s.length - 1`的情况。

Time complexity: O(N)

```java
class Solution {
  public void reverseWords(char[] s) {
    if (s == null || s.length <= 1) {
      return;
    }
    reverse(s, 0, s.length - 1);
    int wordStart = 0;
    for (int i = 0; i < s.length; i++) {
      if (i == s.length - 1 || s[i + 1] == ' ') {
        reverse(s, wordStart, i);
        wordStart = i + 2;
      }
    }
  }

  private void reverse(char[] s, int l, int r) {
    while (l < r) {
      swap(s, l++, r--);
    }
  }

  private void swap(char[] s, int i, int j) {
    char temp = s[i];
    s[i] = s[j];
    s[j] = temp;
  }
}
```