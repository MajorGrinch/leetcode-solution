# 344. Reverse String

给定一个字符数组形式的字符串，把字符串reverse一下。好像没什么好说的。。。

```java
class Solution {
  public void reverseString(char[] s) {
    if (s == null || s.length <= 1) return;
    int l = 0, r = s.length - 1;
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