# 3. Longest Substring Without Repeating Characters

给定一个有重复字符的字符串，找到最长的无重复字符的子串，并输出其长度。

这题是sliding window的经典题型。用start表明目前无重复子串的起始位置，每一轮循环到达最后一步时我们都保证`[start, i]`是无重复字符的子串。

为了在每一轮结束之前维护这一性质，我们需要一个lastOccur数组来记录所有字符上一次出现的位置。假如当前处理的字符上一次出现在start或者之后，那么我们就需要更新start为该字符上一次出现的位置的后一位，以保持`[start, i]`是无重复字符的性质。维护好这一性质之后，我们就可以知道以`i`结尾的无重复字符的子串长度，和以前遇到过的最长的作对比。

Time complexity: O(N), we only access every character once.

Space complexity: O(1), constant space is needed.

```java
class Solution {
  public int lengthOfLongestSubstring(String s) {
    if (s == null || s.length() == 0) {
      return 0;
    }
    int[] lastOccur = new int[256];
    Arrays.fill(lastOccur, -1);
    int start = 0, maxL = 0;
    for (int i = 0; i < s.length(); i++) {
      char c = s.charAt(i);
      if(lastOccur[c] >= start) {
        start = lastOccur[c] + 1;
      }
      lastOccur[c] = i;
      maxL = Math.max(maxL, i - start + 1);
    }
    return maxL;
  }
}
```