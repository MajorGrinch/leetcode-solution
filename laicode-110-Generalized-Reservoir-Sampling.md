# Laicode 110. Generalized Reservoir Sampling

给定无限的流输入，实现读入接口和从已读入数据里随机抽k个的接口。

这题和[laicode 109. Reservoir Sampling](laicode-109-Reservoir-Sampling.md)很像，只不过是从单个抽样变成多个。

那么我们就用一个list来记录被我们抽中的样本，用`count`来记录当前已经读入多少个数据了。当小于等于k个时，所有的数据都会被抽样拿走。当大于k个时，每个元素被抽中的概率应该是`k/count`。那么我们用`nextInt(count)`来随机生成`[0, count)`的整数idx。只要`idx`小于`k`，那么我就替换List里下标`idx`地方的为当前读入的数据。这样所有元素被抽样的概率都是`k/count`了。

```java
public class Solution {
  private final int k;
  private Random rand;
  private List<Integer> res;
  private int count = 0;

  public Solution(int k) {
    this.k = k;
    rand = new Random();
    res = new ArrayList<>();
  }

  public void read(int value) {
    count++;
    if (count <= k) {
      res.add(value);
    } else {
      int idx = rand.nextInt(count);
      if (idx < k) {
        res.set(idx, value);
      }
    }
  }

  public List<Integer> sample() {
    return res;
  }
}
```