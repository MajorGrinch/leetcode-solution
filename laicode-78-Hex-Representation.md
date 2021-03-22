# Laicode 78. Hexadecimal Representation

给定一个十进制数，把它变成十六进制的形式，输出字符串。

这个好像没啥好说的，就和转二进制一样。不过最后要把结果反转一下再输出，因为最低位要求在最右边。

Time complexity: O(logn)

Space complexity: O(1)

```java
public class Solution {
  public String hex(int number) {
    if (number == 0) {
      return "0x0";
    }
    char[] hexChars = {
      '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F'
    };
    String prefix = "x0";
    StringBuilder hexBuilder = new StringBuilder();
    while (number > 0) {
      hexBuilder.append(hexChars[number % 16]);
      number /= 16;
    }
    hexBuilder.append(prefix);
    return hexBuilder.reverse().toString();
  }
}
```