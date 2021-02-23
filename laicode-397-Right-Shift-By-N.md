# Laicode 397. Right Shift By N Characters

给定一个字符串，循环右移N位。

Clarification:
+ n保证不小于0吗？保证
+ n保证小于字符串长度吗？不保证

假设字符串长度为`l`，如果`n == l`，其实和没有右移一样。所以我们上来先把n % l，来看实际上需要右移多少位。

我们真的需要右移嘛？不，我们观察一下`abcdefg`右移两位变成`fgabcde`。那么其实我们可以先把整体反转成为`gfedcba`，然后`[0, 1]`和`[2, l)`各自反转一下，就好了。所以其实这里就是整体先反转，然后`[0, n - 1]和[n, l)各自反转，就可以达到右移n位的目的。

```java
public class Solution {
  public String rightShift(String input, int n) {
    if(input == null || input.length() <= 1){
      return input;
    }
    char[] sc = input.toCharArray();
    n %= sc.length;
    reverse(sc, 0, sc.length - 1);
    reverse(sc, 0, n - 1);
    reverse(sc, n, sc.length - 1);
    return new String(sc);
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