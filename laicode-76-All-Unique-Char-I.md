# Laicode 76. All Unique Characters I

和[laicode 77. All Unique Characters II](laicode-77-All-Unique-Chars-II.md)差不多，但是这题更简单。因为输入的字符只有26个小写字母，所以我们只要用一个`int`就可以记录下各个字母是否出现过。

Time complexity: O(n)

Space complexity: O(1)

```java
public class Solution {
  public boolean allUnique(String word) {
    int mask = 0;
    for (int i = 0; i < word.length(); i++) {
      char c = word.charAt(i);
      int num = 1 << (c - 'a');
      if ((mask & num) > 0) return false;
      mask |= num;
    }
    return true;
  }
}
```