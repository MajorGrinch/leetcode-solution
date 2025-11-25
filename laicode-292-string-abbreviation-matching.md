# Laicode 292. String Abbreviation Matching

给定一个字符串和一个缩写，判定缩写是不是正确的。

其实就是双指针，每次在缩写里面无非就是这几种情况
+ 当前字符是数字，那么就把这个数字x读完，并且原字符串里面对应跳过x位
+ 当前数字是字母，那么就和原字符串指针对应位置的字母对比。不一致就返回false，一致就继续往后对比。

有一个情况需要注意，我的while循环只用缩写字符串作为判定，所以我得保证原字符串的那个指针始终不越界，而且最后结束的时候也正好停在原字符串结尾之后的位置。

```java
public class Solution {
  public boolean match(String input, String pattern) {
    char[] inputArr = input.toCharArray();
    char[] patArr = pattern.toCharArray();
    int i = 0;
    int j = 0;
    while(j < patArr.length) {
      if(i >= inputArr.length) {
        return false;
      }
      if(isDigit(patArr[j])) {
        int num = 0;
        while(j < patArr.length && isDigit(patArr[j])) {
          num = num * 10 + patArr[j] - '0';
          j++;
        }
        i += num;
      } else {
        // patArr[j] is not number
        if(inputArr[i] == patArr[j]) {
          i++;
          j++;
        } else {
          return false;
        }
      }
    }
    return i == inputArr.length;
  }

  public boolean isDigit(char c) {
    return '0' <= c && c <= '9';
  }
}
```

2025 code:

```java
public class Solution {
  public boolean match(String input, String pattern) {
    int i = 0;
    int j = 0;
    while(i < input.length() && j < pattern.length()) {
      if(isDigit(pattern.charAt(j))) {
        int count = 0;
        while(j < pattern.length() && isDigit(pattern.charAt(j))) {
          count = count * 10 + pattern.charAt(j) - '0';
          j++;
        }
        i += count;
      } else {
        if(input.charAt(i) != pattern.charAt(j)) {
          return false;
        }
        i++;
        j++;
      }
    }
    return i == input.length() && j == pattern.length();
  }

  private boolean isDigit(char c) {
    return '0' <= c && c <= '9';
  }
}
```