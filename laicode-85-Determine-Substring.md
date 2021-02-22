# Laicode 85. Determine If One String Is Another's Substring

给定两个字符串，`large`和`small` 判断第二个是否是第一个的子串。

Clarification:
+ 两个输入为null怎么办？题目保证不为null
+ small是空串怎么办？返回0

这里用`Rabin-Karp`子串方法来判定`small`是否是`large`的子串。当然也有更优秀的KMP算法，但是KMP实在太难了，面试时候容易卡住，~~流下了不学无术的泪水~~。

`Rabin-Karp`的思想在于比较large的子串和`small`的时候先利用哈希值来进行初步判定，如果哈希值都不一样，那这个子串必然和`small`是不一样的。如果哈希值一样，那就进一步比较看看是否真的一样。

我们先把`large`和`small`都先变成char array，方便我们控制。getSubstrHash可以计算字符数组给定起始点相应长度子串的哈希值，其实就是模拟26进制。但是如果我们把`large`里每个`small`长度的子串都用`getSubstrHash`计算出来的话，算法复杂度就是(MN)了。所以这里我们需要一个接近O(1)复杂度的哈希算法，以减少复杂度。

这就需要`rolling hash`哈希算法，用`sc[i - 1, i - 1 + len)`来辅助计算`sc[i, i + len)`子串的哈希值，从而达到常量级时间复杂度内计算出`sc[i, i + len)`。用前一个子串的哈希值，减去sc[i-1]，结果乘以base，再加上新的字符和`a`的差值，就可以得到`sc[i, i + len)`的哈希值。

所以我们这题先计算出`large[0, small.length)`的哈希值，然后后面所有子串的哈希值都由前一个子串的哈希值来计算得到。

Time complexity: O(M + N) as expected, O(MN) in worst case if the hash function is poor. O(M + N) because if the hash collision is few, we only access each character in each string only once.

```java
public class Solution {
  public int strstr(String large, String small) {
    if (small.length() == 0) return 0;
    if (small.length() > large.length()) return -1;
    char[] sc = large.toCharArray(), tc = small.toCharArray();
    int smallHash = getSubstrHash(tc, 0, tc.length);
    int prevHash = 0;
    int multiplier = 1;
    for(int i = 0; i < tc.length - 1; i++) multiplier *= 26;
    for (int i = 0; i < sc.length - tc.length + 1; i++) {
      int currHash =
          (i == 0
              ? getSubstrHash(sc, 0, tc.length)
              : rollingHash(prevHash, multiplier, sc[i - 1], sc[i + tc.length - 1]));
      if (currHash == smallHash) {
        boolean equal = true;
        for (int j = 0; j < tc.length; j++) {
          if (sc[i + j] != tc[j]) equal = false;
        }
        if (equal) return i;
      }
      prevHash = currHash;
    }
    return -1;
  }

  /** Given a char array, return the hash value of [start, start + offset) */
  private int getSubstrHash(char[] sc, int start, int offset) {
    int hash = 0;
    for (int i = start; i < start + offset; i++) {
      hash = (hash * 26 + sc[i] - 'a');
    }
    return hash;
  }

  /**
   * Given a previous substring hash value, calculate the current substring hash value in constant
   * time.
   */
  private int rollingHash(int previous, int multiplier, char oldChar, char newChar) {
    int base = 26;
    return (previous - (oldChar - 'a') * multiplier) * base + (newChar - 'a');
  }
}
```