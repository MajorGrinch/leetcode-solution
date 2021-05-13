# Laicode 82. Remove Adjacent Repeated Characters IV

给定一个字符串，不断地删去连续重复的字符，不保留任何连续的字符。大概类似于祖玛游戏那样，`abba`先删去中间的`bb`，删完之后`aa`相邻了，就接着删`aa`。

双指针同时从0出发，fast代表当前要处理的位置，[0, slow)记录着已经处理好的没有连续重复字符的子串。
+ 如果slow > 0 且 fast和slow - 1位置的字符一样的话，说明`fast`及其相邻的重复字符需要和`slow - 1`需要相消。
+ 如果slow为0，或者fast和slow - 1位置不一样，就老老实实把fast加入已经处理好的子串，slow后移。

相当于如果我们每个字符段都是至少把它首个字符压入[0, slow)，如果这个字符段只有一个字符，那么就成功压入。如果这个字符段不止一个字符，那么下一步会跳过后面所有的并且弹出一开始压入的首个字符。

Time complexity: O(n). Why? Because we only process each character once.

Space complexity: O(n), because of the extra char array.

```java
public class Solution {
  public String deDup(String input) {
    if (input == null || input.length() <= 1) {
      return input;
    }
    char[] sc = input.toCharArray();
    int slow = 0, fast = 0;
    while (fast < sc.length) {
      if (slow > 0 && sc[fast] == sc[slow - 1]) {
        while (fast < sc.length && sc[fast] == sc[slow - 1]) fast++;
        slow--;
      } else {
        sc[slow++] = sc[fast++];
      }
    }
    return new String(sc, 0, slow);
  }
}
```