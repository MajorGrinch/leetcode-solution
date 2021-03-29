# Laicode 111. Random7 Using Random5

给定一个random5()的接口可以随机生成`[0, 5)`的整数，实现一个random7()的接口去随机生成`[0, 7)`的整数。

直观地来说，我们会想到用两个random5相加的结果模除7来实现。但是这个方法只要仔细一想就知道不对，比如生成2的组合是`0,2`、`2,0`、`1,1`、`4,4`，那么概率就是`4/25`，显然概率和其他数字并不相等。所以说，这个方法生成的并不是真正随机概率。

我们首先需要实现的就是随机生成的更大范围的数。如何利用已给的接口生成范围更大的随机数呢？利用数位的原理，用random5()实现两位5进制数，实现0-24的随机数生成方法。然后只要得到小于21的数，就模除7，就可以返回一个均匀的`[0, 7)`的随机数了。

```java
public class Solution {
  public int random7() {
    // you can use RandomFive.random5() for generating
    // 0 - 4 with equal probability.
    while (true) {
      int res = random25();
      if (res < 21) {
        return res % 7;
      }
    }
  }

  private int random25() {
    return RandomFive.random5() * 5 + RandomFive.random5();
  }
}
```