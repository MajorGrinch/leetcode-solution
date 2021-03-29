# Laicode. 108. Perfect Shuffle

给定一个数组进行随机shuffle。

很简单，从头开始对于每个下标从后面位置里任意挑一个和它进行交换，这样就可以取得对整个数组进行随机shffle的效果了。

```java
public class Solution {
  public void shuffle(int[] array) {
    Random rand = new Random();
    int n = array.length;
    for (int i = 0; i < n; i++) {
      int j = rand.nextInt(n - i) + i;
      swap(array, i, j);
    }
  }

  private void swap(int[] array, int i, int j) {
    int temp = array[i];
    array[i] = array[j];
    array[j] = temp;
  }
}
```