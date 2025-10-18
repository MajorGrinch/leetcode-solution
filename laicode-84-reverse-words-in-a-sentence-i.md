# Laicode 84. Reverse words in a sentence I

把一个句子里的所有单词翻转过来。no leading space, trailing space and only one space between words

很简单，先把整个string反转一下，然后每个单词再单独翻转一下，就可以实现我们要的效果。

遍历的时候，记录单词的起点，遇到空格就说明单词结束了，翻转单词。然后再去下一个单词。


```java
public class Solution {
  public String reverseWords(String input) {
    if(input == null || input.length() <= 1) {
      return input;
    }
    char[] sc = input.toCharArray();
    reverse(sc, 0, sc.length - 1);  // reverse the whole string
    int fast = 0;
    while(fast < sc.length) {
      int begin = fast;
      while(fast < sc.length && sc[fast] != ' ') {
        fast++;
      }
      reverse(sc, begin, fast - 1);
      fast++; // move to next word
    }
    return new String(sc);
  }

  private void reverse(char[] sc, int l, int r) {
    while(l < r) {
      swap(sc, l++, r--);
    }
  }

  private void swap(char[] sc, int i, int j) {
    char temp = sc[i];
    sc[i] = sc[j];
    sc[j] = temp;
  }
}
```