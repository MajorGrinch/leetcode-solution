# Laicode 81. Remove Adjacent Repeated Characters III

和[laicode 79. Remove Adjacent Repeated Characters I](laicode-79-Remove-Adjacent-Repeated-Chars-I.md)差不多，但是相邻的相同字符一个不留。这题依然采用快慢双指针的方式，我们把字符串分块。块内字符都是相同的，块的长度至少为1。那么`abbbcccd`可以分为四块，`a`、`bbb`、`ccc`和`d`。

同理，我们每次遍历一个块，`begin`停在头部，`fast`往后走到直至和`begin`指向的字符不一样为止以丈量块的长度。长度为1就加入`[0, slow)`，否则就跳过。

Time complexity: O(n)

Space complexity: O(1)

```java
public class Solution {
  public String deDup(String input) {
    if (input == null || input.length() <= 1) {
      return input;
    }

    int slow = 0, fast = 0;
    char[] sc = input.toCharArray();
    while (fast < sc.length) {
      int begin = fast;
      while (fast < sc.length && sc[fast] == sc[begin]) fast++;
      if (fast - begin > 1) continue;
      sc[slow++] = sc[begin];
    }
    return new String(sc, 0, slow);
  }
}
```