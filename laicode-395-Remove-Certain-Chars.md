# Laicode 395. Remove Certain Characters

给定一个字符串input和一个字符串t，从input里删掉所有t里出现过的字符。

这题需要用到HashSet来记录该字符是否需要被删，用slow指针来记录答案。[0, slow)位置始终记录着被删减之后的字符串。

先把字符串转换为字符数组，然后逐个字符遍历。遍历的时候，如果当前字符无需删减，就放到slow的位置，slow后移。如果当前字符需要删减，就什么也不做。这样，最后[0, slow)存的就是被删减过后的字符。

Time complexity: O(n)

Space complexity: O(n)

```java
public class Solution {
  public String remove(String input, String t) {
    Set<Character> delSet = new HashSet<>();
    for (char c : t.toCharArray()) delSet.add(c);
    char[] sc = input.toCharArray();
    int slow = 0;
    for (int i = 0; i < sc.length; i++) {
      if (!delSet.contains(sc[i])) sc[slow++] = sc[i];
    }
    return new String(sc, 0, slow);
  }
}
```