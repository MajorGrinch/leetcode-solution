# Laicode 99. Dictionary Word I

参照[leetcode 139](139-Word-Break.md)。

```java
public class Solution {
  public boolean canBreak(String input, String[] dict) {
    Set<String> dictSet = new HashSet<>();
    for(String word: dict) {
      dictSet.add(word);
    }
    int len = input.length();
    boolean[] breakable = new boolean[len + 1];
    breakable[0] = true;
    for(int i = 1; i < len + 1; i++) {
      for(int j = 0; j < i; j++) {
        if(breakable[j] && dictSet.contains(input.substring(j, i))) {
          breakable[i] = true;
          break;
        }
      }
    }
    return breakable[len];
  }
}
```