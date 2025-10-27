# Laicode 225. String to Integer



```java
public class Solution {
  public int atoi(String str) {
    if(str == null || str.length() == 0) {
      return 0;
    }
    char[] sc = str.toCharArray();
    boolean positive = true;
    int i = 0;
    while(i < sc.length && sc[i] == ' ') {
      // skip leading space
      i++;
    }
    if(i == sc.length) {
      return 0;
    }

    if(sc[i] == '+') {
      i++;
    } else if(sc[i] == '-') {
      positive = false;
      i++;
    }

    long res = 0;

    while(i < sc.length && isDigit(sc[i])) {
      res = res * 10 + sc[i] - '0';
      i++;
    }

    if(positive) {
      return res > Integer.MAX_VALUE ? Integer.MAX_VALUE : (int)res;
    } else {
      res = -res;
      return res < Integer.MIN_VALUE ? Integer.MIN_VALUE : (int)res;
    }
  }

  private boolean isDigit(char c) {
    return '0' <= c && c <= '9';
  }
}
```