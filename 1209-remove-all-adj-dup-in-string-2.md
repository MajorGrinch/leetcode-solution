# 1209. Remove All Adjacent Duplicates in String II

和[leetcode 1047](1047-remove-all-adj-dup-in-string.md)差不多，只不过从消除相邻的2个变成了消除相邻的 K 个。

还是通用解法，`[begin, fast)`的个数为 count，`count % k`就是要保留的个数 keep。当 keep > 0 的时候，还是根据 `slow == 0 || sc[slow - 1] != sc[begin]` 来看这些字符是否可以直接加入`[0, slow)`这个已经处理好的范围。如果不满足的话，说明什么？说明 `sc[slow - 1] == sc[begin]`，那么我们就要开始相消了。从 `slow - 1` 开始向前一直找相同字符的个数，计算相消之后还能剩的个数，顺带把 `slow`挪到前面。还能剩 keep 个，就从 slow 的新地方开始加入。

Time complexity: O(N)

Space complexity: O(N)

```java
class Solution {
    public String removeDuplicates(String s, int k) {
        if (s == null || s.length() < k) {
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
            int keep = count % k;
            if (keep > 0) {
                if (slow == 0 || sc[slow - 1] != sc[begin]) {
                    for (int i = 0; i < keep; i++) {
                        sc[slow++] = sc[begin];
                    }
                } else {
                    // sc[slow - 1] == sc[begin]
                    int tSlow = slow - 1;
                    while (tSlow >= 0 && sc[tSlow] == sc[slow - 1]) {
                        tSlow--;
                    }
                    keep += slow - 1 - tSlow;
                    keep %= k;
                    slow = tSlow + 1;
                    if (keep > 0) {
                        for (int i = 0; i < keep; i++) {
                            sc[slow++] = sc[begin];
                        }
                    }
                }
            }
        }
        return new String(sc, 0, slow);
    }
}
```