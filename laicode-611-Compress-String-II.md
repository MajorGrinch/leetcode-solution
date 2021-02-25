# Laicode 611. Compress String II

对字符串进行压缩，连续的相同字符压缩成`字符+数字`的形式。比如`abbcccdeee" -> "a1b2c3d1e3"。

分两轮遍历，第一轮遍历的时候先处理两个及以上的字母。双指针`slow`和`fast`，`[0, slow)`存的是已经处理好的部分，`fast`是当前正在处理的下标。每当fast后面有和它一样的字符时，fast就不断后移，最后看看到底有几个。如果只有一个的话，那么这个字符x最后会变成`x1`，所以被压缩之后长度反而变成了2。如实拷贝下`x`，并且新长度加2。如果x有多个话，那就如实拷贝x，并且拷贝个数。压缩之后的长度，变成了数字的长度加上这个字符的一个长度，slow指针同时往后移个数的位数次。

经过上一轮遍历，数组基本可以变成`ab2c3de3`。现在我们已经知道压缩后新字符串的长度了，新开个字符数组，然后直接把目前的字符串从后向前遍历。遇到了数字y，就拷贝y个y之前的那个字符，并且跳过这俩。如果遇到了字符x，就拷贝`x1`。这样最后字符串就变成了`a1b2c3d1e3`。

Time complexity: O(n). 两轮遍历。

```java
public class Solution {
  public String compress(String input) {
    if(input.length() == 0) return input;
    return compress(input.toCharArray());
  }

  private String compress(char[] sc) {
    int slow = 0, fast = 0;
    int newLen = 0;
    while (fast < sc.length) {
      int begin = fast;
      while (fast < sc.length && sc[fast] == sc[begin]) fast++;
      sc[slow++] = sc[begin];
      if (fast - begin == 1) {
        newLen += 2;
      } else {
        int countLen = copyDigits(sc, slow, fast - begin);
        newLen += countLen + 1;
        slow += countLen;
      }
    }
    char[] tc = new char[newLen];
    int i = slow - 1, j = tc.length - 1;
    while (i >= 0) {
      if (Character.isDigit(sc[i])) {
        while (i >= 0 && Character.isDigit(sc[i])) {
          tc[j--] = sc[i--];
        }
      } else {
        tc[j--] = '1';
      }
      tc[j--] = sc[i--];
    }
    return new String(tc);
  }

  private int copyDigits(char[] sc, int start, int num) {
    int len = 0;
    for(int i = num; i > 0; i /= 10) {
      start++;
      len++;
    }
    for (int i = num; i > 0; i /= 10) {
      char digit = (char) (i % 10 + '0');
      sc[--start] = digit;
    }
    return len;
  }
}
```