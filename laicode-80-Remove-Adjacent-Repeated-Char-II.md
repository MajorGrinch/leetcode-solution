# Laicode 80. Remove Adjacent Repeated Characters II

给定一个字符串，消除掉相邻的相同字符，最多允许两个相同字符相邻。和[laicode 79. Remove Adjacent Repeated Characters I](laicode-79-Remove-Adjacent-Repeated-Chars-I.md)差不多，但是可以允许两个相同的相邻字符。还是快慢指针思想，但是这次我们不仅检查`slow-1`还检查`slow-2`，如果`sc[i] == sc[slow - 1] == sc[slow - 2]`，那么就已经有两个相邻的相同字符了，`sc[i]`就不要加入`[0, slow)`了。否则，`sc[i]`就可以加入。

Time complexity: O(n)

Space complexity: O(1)

```java
public class Solution {
  public String deDup(String input) {
    if (input.length() <= 2) {
      return input;
    }
    char[] sc = input.toCharArray();
    int slow = 2;
    for (int i = 2; i < sc.length; i++) {
      if (sc[slow - 2] == sc[i] && sc[slow - 1] == sc[i]) continue;
      sc[slow++] = sc[i];
    }
    return new String(sc, 0, slow);
  }
}
```

受[laicode 81. Remove Adjacent Repeated Characters III](laicode-81-Remove-Adjacent-Repeated-Char-III.md)启发，也可以这么写。时空复杂度不变。

```java
public class Solution {
  public String deDup(String input) {
    if (input.length() <= 2) {
      return input;
    }
    char[] sc = input.toCharArray();
    int slow = 0, fast = 0;
    while (fast < sc.length) {
      int begin = fast;
      while (fast < sc.length && sc[fast] == sc[begin]) fast++;
      for (int i = 0; i < 2 && begin < fast; i++) sc[slow++] = sc[begin++];
    }
    return new String(sc, 0, slow);
  }
}
```