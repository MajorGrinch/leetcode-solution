# 139. Word Break

经典DP题，给定一个字符串`s`和一个单词数组，计算`s`是否可以由数组里的单词拼接而得。

我们用`breakable`来存放子问题答案，`breakable[i]`表示`[0, i)`子串是否可以由给定单词拼接得到。那么显然，只要任意的`j`满足`0 <= j < i`，`breakable[j] == true`而且`s.substring(j, i)`出现在了单词数组里，那么`[0, i)`子串就可以由给定的单词数组拼接得到。

`breakable[0]`表示`[0, 0)`子串，空串当然是可以的，所以为`true`。从1开始，一直到`s.length()`这样才可以涵盖整个字符串。对于每个小于`i`的`j`进行遍历，只要满足之前的条件，那么说明`[0, i)`是可以拆分得到单词的。一直到最后，那么`breakable[s.length()]`就代表整个字符串是否可以。

Time complexity: O(N^3)，substring方法算作O(N)。

Space Complexity: O(M + N), where N is the string length and M is the dictionary length.

```java
class Solution {
  public boolean wordBreak(String s, List<String> wordDict) {
    Set<String> wordSet = new HashSet<>(wordDict);
    boolean[] breakable = new boolean[s.length() + 1];
    breakable[0] = true;
    for (int i = 1; i <= s.length(); i++) {
      for (int j = 0; j < i; j++) {
        if (breakable[j] && wordSet.contains(s.substring(j, i))) {
          breakable[i] = true;
          break;
        }
      }
    }
    return breakable[s.length()];
  }
}
```

2025 code:

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> dict = new HashSet<>();
        for (String word : wordDict) {
            dict.add(word);
        }
        boolean[] breakable = new boolean[s.length() + 1];
        breakable[0] = true;
        for (int i = 1; i < s.length() + 1; i++) {
            for (int j = 0; j < i; j++) {
                if (breakable[j] && dict.contains(s.substring(j, i))) {
                    breakable[i] = true;
                }
            }
        }
        return breakable[s.length()];
    }
}
```