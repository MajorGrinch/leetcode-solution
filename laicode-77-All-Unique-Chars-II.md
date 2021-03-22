# Laicode 77. All Unique Characters II

# Map Approach

给定一个只有ASCII码组成的字符串，判断是否有重复字符。

既然说了全是ASCII码，那么用一个长度为256的数组充当hashmap就好，直接判断。

```java
public class Solution {
  public boolean allUnique(String word) {
    int[] occur = new int[256];
    for (char ch : word.toCharArray()) {
      if (occur[ch] > 0) {
        return false;
      }
      occur[ch] = 1;
    }
    return true;
  }
}
```

Time complexity: O(n)

Space complexity: O(1)

## Optimized Space

以上方法用了256长度的int数组，我们可以更加解决空间吗？答案是可以。256的字符范围可以被8个长度为32的int来表示，数组长度可以被进一步压缩到8。

拿到当前字符的ASCII码之后，除以32决定它放在第几个数字，模除32决定它在第几位。只要那个数字的那一位不为1，说明之前没有出现过，将此位置一。否则就是出现过，返回false。

```java
public class Solution {
  public boolean allUnique(String word) {
    int[] occur = new int[8];
    for (char ch : word.toCharArray()) {
      int row = ch / 32, col = ch % 32;
      if(((occur[row] >> col) & 1) == 1){
        return false;
      }
      occur[row] |= 1 << col;
    }
    return true;
  }
}
```