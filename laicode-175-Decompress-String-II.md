# Laicode. 175 Decompress String II

给定压缩后的字符串，对其进行解压。比如`a1c0b2c4`变成`abbcccc`。

Clarification:
+ 输入是否可能为null？不
+ 字符串的字符范围？仅小写字母
+ 数字范围？0-9

那么这里我们依然两步走，第一步先解压所有数字小于等于2的，第二步解压大于2的。因为数字小于等于2的话，解压出来最多和压缩之后等长，而不会超过，所以可以在原数组上进行解压。因此就把`a1c0b2c4`变成`abbc4`。那么我们可以从前向后，每次遍历两位。先看数字是否在0~2以内，如果是的话就解压。不是的话，就拷贝。这样我们就可以初步得到解压所有0~2之后的字符串长度。

紧接着，第二部我们解压所有数字大于2的。那么我们先要得到解压后新字符串的长度，依次遍历所有剩余的数字，拿上一步得到的长度加上每个数字减2，就是解压缩之后的新长度。双指针，i从初步解压缩的最后一位向前，j从新字符数组的最后一位向前。每当i遇到一个数字`y`，就把它前面的字符拷贝`y`份到以j为结尾的地方。i要跳过前面的字符，j要跳过`y`个长度向前。如果i遇到字母，那就简单的拷贝i给j。

这样最后整个字符串就被我们成功解压缩了。

Time complexity: O(n)，三轮遍历。

```java
public class Solution {
  public String decompress(String input) {
    char[] sc = input.toCharArray();
    return decodeLong(sc, decodeShort(sc));
  }

  private int decodeShort(char[] sc) {
    int end = 0;
    for (int i = 0; i < sc.length; i += 2) {
      int digit = sc[i + 1] - '0';
      if (0 <= digit && digit <= 2) {
        fillChars(sc, sc[i], end, digit);
        end += digit;
      } else {
        sc[end++] = sc[i];
        sc[end++] = sc[i + 1];
      }
    }
    return end;
  }

  private String decodeLong(char[] sc, int len) {
    int newLen = len;
    for (int i = 0; i < len; i++) {
      int digit = sc[i] - '0';
      if (3 <= digit && digit <= 9) {
        newLen += digit - 2;
      }
    }
    char[] tc = new char[newLen];
    int j = newLen - 1;
    for (int i = len - 1; i >= 0; i--) {
      int digit = sc[i] - '0';
      if (3 <= digit && digit <= 9) {
        fillChars(tc, sc[--i], j - digit + 1, digit);
        j -= digit;
      } else {
        tc[j--] = sc[i];
      }
    }
    return new String(tc);
  }

  private void fillChars(char[] sc, char c, int start, int amount) {
    for (int i = 0; i < amount; i++) {
      sc[start + i] = c;
    }
  }
}
```