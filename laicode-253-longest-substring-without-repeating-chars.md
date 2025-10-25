# Laicode 253. Longest substring without repeating characters

参考[Leetocde 3](3-Longest-Substring-Without-Repeating-Chars.md)

```java
public class Solution {
  public int longest(String input) {
    if(input == null || input.length() == 0) {
      return 0;
    }
    int[] lastOccur = new int[256];
    Arrays.fill(lastOccur, -1);
    int subStart = 0;
    int longest = 0;
    for(int i = 0; i < input.length(); i++) {
      char curr = input.charAt(i);
      if(lastOccur[curr] >= subStart) {
        subStart = lastOccur[curr] + 1;
      }
      longest = Math.max(longest, i - subStart + 1);
      lastOccur[curr] = i;
    }
    return longest;
  }
}
```