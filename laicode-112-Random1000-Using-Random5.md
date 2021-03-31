# Laicode 112. Random1000 Using Random5

给定`random5()`方法随即返回`0-4`的数，实现一个random1000方法返回`[0, 1000)`的数。

这题的思路和之前的[laicode 111. Random7 Using Random5](laicode-111-Random7-Using-Random5.md)差不多，这里我们要想利用`random5()`来等概率的生成更大范围的数，就需要利用进制。给的是`random5()`我们就用五进制，随机生成`[0, 5^5)`范围内的数字。这个范围内的每一个数字概率都是一样的，所以我们要向得到`[0, 1000)`范围，就直接把`[0, 3000)`范围对3进行模除，从而使得每个`[0, 1000)`里面的数都是`3/3215`的概率。

```java
public class Solution {
  public int random1000() {
    while (true) {
      int res = random5PowK(5);
      if (res < 3000) {
        return res % 1000;
      }
    }
  }

  private int random5PowK(int k) {
    int val = 0;
    for (int i = 0; i < k; i++) {
      val = val * 5 + RandomFive.random5();
    }
    return val;
  }
}
```