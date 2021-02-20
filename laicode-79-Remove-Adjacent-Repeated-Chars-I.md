# Laicode 79 Remove Adjacent Repeated Characters I

给定一个字符串，所有连续的重复字符只留下一个。

Clarification:
+ input为null怎么办？
+ 返回String还是char array？

还是双指针思想，但是这次双指针同时从1出发，因为下标0的字符肯定是要被保留下来的。slow保持的性质是[0, slow)存的是去重之后的子串。

从1出发，如果当前字符和前一个字符一样，说明是连续重复字符，那就不加入[0, slow)。否则，就加入[0, slow)并且slow后移。

Time complexity: O(n)

Space complexity: O(n), because of the extra char array we use.

```java
public class Solution {
  public String deDup(String input) {
    if(input == null || input.length() <= 1){
      return input;
    }
    char[] sc = input.toCharArray();
    int slow = 1;
    for(int i = 1; i < sc.length; i++){
      if(sc[i] != sc[i - 1]){
        sc[slow++] = sc[i];
      }
    }
    return new String(sc, 0, slow);
  }
}
```