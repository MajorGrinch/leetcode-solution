# Laicode 109. Reservoir Sampling

给定给一个无限的流输入，实现读入数据和随机抽样的接口。

因为是无线的流输入，所以我们不可能记录下每个输入然后从中随机抽样，这就需要我们另辟蹊径来进行随机抽样。

既然是随机抽样，那么到目前为止的所有输入都应该有相同的概率被抽中。因此，我们用一个`count`变量来记录到目前为止读了多少数据，`res`来记录随机抽样返回的数据。读入新数据之后，用`1/count`的概率来把它赋值给`res`。这样我们就可以做到对于已经读入的数据，他们被抽样的概率都是相等的。

```java
public class Solution {

  private int count = 0;
  private Random rand;
  private Integer res;

  public Solution() {
    rand = new Random();
    res = null;
  }

  public void read(int value) {
    count++;
    if (rand.nextInt(count) == 0) {
      res = value;
    }
  }

  public Integer sample() {
    return res;
  }
}
```