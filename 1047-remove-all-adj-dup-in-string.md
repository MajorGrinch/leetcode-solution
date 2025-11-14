# 1047. Remove All Adjacent Duplicates In String

和[laicode 82](laicode-82-Remove-Adjacent-Repeated-Characters-IV.md)相似，但是略有区别。这题要求的是除去相邻的重复字符。比如 `aaa` 会变成 `a` 而不是一味的相消变成空字符串。`abba` 会变成空串，因为`bb`消掉之后`aa`也会消掉。`abbba`会变成`aba`因为`bbb`会变成`b`。

那么这题还是通用解法，但是处理上不一样。`[begin ,fast)` 有偶数个元素的时候就全都相消了，有奇数个元素的时候才会剩一个。那么我们只对奇数的情况进行处理，当 slow == 0 或者 sc[slow - 1] != sc[begin]的时候，sc[begin]才能放入 `[0, slow)` 里面。反过来，sc[slow - 1] == sc[begin]的时候，这俩就会相消，那么 slow--，sc[begin]不放入 `[0, slow)`。

Time complexity: O(N)

Space complexity: O(N)

```java
class Solution {
    public String removeDuplicates(String s) {
        if (s == null || s.length() <= 1) {
            return s;
        }
        int slow = 0;
        int fast = 0;
        char[] sc = s.toCharArray();
        while (fast < sc.length) {
            int begin = fast;
            while (fast < sc.length && sc[fast] == sc[begin]) {
                fast++;
            }
            int count = fast - begin;
            if (count % 2 == 1) {
                if (slow == 0 || sc[slow - 1] != sc[begin]) {
                    sc[slow++] = sc[begin];
                } else {
                    slow--;
                }
            }
        }
        return new String(sc, 0, slow);
    }
}
```