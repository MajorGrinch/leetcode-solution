# 44. Wildcard Matching

这题很难需要好好解释一下。

我们采取双指针的思想，用 i 和 j 两个指针在两个字符串里面走。额外用 lastStarIdx 和 lastStarCover 来记录最新的 `*` 的坐标和覆盖范围，物理意义是 `[0, lastStarCover)` 是已经处理好的部分。遍历的时候，按照顺序考虑以下几种情况：
1. `s[i] == p[j]`，那说明两个字符串这个位置字符是一样的，双指针同时往后一步。
2. `p[j] == '?'`，既然是通用单个字符，和情况 1 相同。
3. `p[j] == '*'`，既然是通用多字符，我们需要多种情况。
    + 默认情况下，我们给 `*` 匹配为空串。比如， `a*a` ==> `aa`。
    + 现实大多数时候，我们需要匹配多个字符。比如，`a*a` ==> `abcda`。 
    + 所以我们这一步就先用 lastStarIdx 记录 j 的位置，lastStarCover 记录 i 的位置，物理意义是 `[0, i)` 已经处理好了。之后，我们只对 j 往后走一步，i 不动。
4. 不符合上面三个条件，那现在是什么情况？要么 j 走到末尾了，要么 `s[i] != p[j]`。这个时候，我们就需要看看 lastStarIdx 了。如果之前有 `*`，那么我们可以继续扩展那个 `*` 的覆盖范围。lastStarCover 走一步，把 i 调到 lastStarCover 的位置，j 调到 lastStarIdx + 1 的位置。为什么 lastStarCover 是自己走一步？而不是直接跳到当前 `i` 的位置？还要把当前的 `i` 在跳回去？因为 `*` 覆盖的是连续字符串，直接跳过来的话，那么中间跳过的部分万一有和之前其他 `p[j]` 匹配过的，怎么办？原则上我们希望 `*` 能覆盖尽量少的字符，这样就不会影响 `s[i] == p[j]` 这个情况。
5. 如果上面几个条件都不满足，就是 j 走到末尾了，或者 `s[i] != p[j]`，之前也没有 `*` 可以做通用匹配，那么我们可以提前结束了，两个串无法匹配。

按照以上流程遍历结束之后，我们还需要处理 p 后面可能的 trailing star。比如`a*cb**` ==> `aeecb`，pattern 的末尾还有一些 `*`是可以直接跳过的。

Time complexity: O(S + P)

Space complexity: O(1)

```java
class Solution {
    public boolean isMatch(String s, String p) {
        int i = 0;
        int j = 0;
        int lastStarIdx = -1;
        int lastStarCover = -1;
        while (i < s.length()) {
            if (j < p.length() && (s.charAt(i) == p.charAt(j) || p.charAt(j) == '?')) {
                i++;
                j++;
            } else if (j < p.length() && p.charAt(j) == '*') {
                lastStarIdx = j;
                lastStarCover = i;
                j++;
            } else if (lastStarIdx != -1) {
                lastStarCover++;
                i = lastStarCover;
                j = lastStarIdx + 1;
            } else {
                return false;
            }
        }
        while (j < p.length() && p.charAt(j) == '*') {
            j++;
        }
        return j == p.length();
    }
}
```