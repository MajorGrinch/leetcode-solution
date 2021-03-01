# 231. Power of Two

判断一个数是否是2的整数幂。

很简单，2的整数幂用二进制表达的话，就只有一位是`1`，其余位都是0，比如`1000`。那么它减1之后，低位就全被借位成1，`0111`。那么两个数进行与操作，就是0。所以这就是判断一个数是否是2的整数幂的快捷方法。

```java
class Solution {
  public boolean isPowerOfTwo(int n) {
    if (n <= 0) return false;
    return (n & n - 1) == 0;
  }
}
```