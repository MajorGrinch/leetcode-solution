# 438. Find All Anagrams in a String

给定两个字符串`s`和`p`，在`s`中找出所有与`p`异构的子串，并输出所有的子串起始的下标。

Clarification:
+ 字符范围？小写字母
+ s和p的长度？只保证p的长度不为0

sliding window经典题。用一个和p等长的窗口在s里滑动，一开始的窗口是s[0, p.length)。用`sCnt`和`pCnt`来记录s[0, p.length)和p[0, p.length)里各个字符出现的频率。

两个频率数组相减放入delta数组来表示目前窗口和p里各个字符频率的差异，delta[i]为正表示ascii码为i的字符在s[0, p.length)中出现比p多，反之亦然。作为异构，我们当然希望delta数组所有数字都是0的，这样表示两边所有字符的频率都一样。所以我们把delta里所有数字的绝对值相加，如果为0的话，就表示s[0, p.length)正好是一个和p异构的子串。

接着，我们从下标1的地方开始，s[i-1]是刚刚被sliding window排出去的字符l，s[i + p.length - 1]是刚进入window的字符r。鉴于window的内容已经变了，所以对应的`delta[l]`和`delta[r]`也应该改变。在变动之前，先将这俩的绝对值从deltaSum中剔除。delta[l]--，因为window里少了l。delta[r]++，因为window多了个r。再把他俩的绝对值重新加入deltaSum，如果deltaSum为0的话，此时的`s[i, i + p.length - 1]`就和p是异构，把i加入答案。重复此过程，一直到s的最后一个改长度子串。

Time complexity: O(N), we only access every character once.

Space complexity: O(1), only constant space is needed. But here we convert the string to char array, so it's actually O(M + N). We can achieve O(1) space by not converting string to char array and use String.charAt(i) to access each character.

```java
class Solution {
  public List<Integer> findAnagrams(String s, String p) {
    List<Integer> res = new ArrayList<>();
    if (s == null || s.length() == 0 || s.length() < p.length()) {
      return res;
    }
    int[] sCnt = new int[256];
    int[] pCnt = new int[256];
    int[] delta = new int[256];
    int deltaSum = 0;
    char[] sc = s.toCharArray();
    char[] pc = p.toCharArray();
    for (int i = 0; i < pc.length; i++) {
      sCnt[sc[i]]++;
      pCnt[pc[i]]++;
    }
    for (int i = 0; i < sCnt.length; i++) {
      delta[i] = sCnt[i] - pCnt[i];
      deltaSum += Math.abs(delta[i]);
    }
    if (deltaSum == 0) {
      res.add(0);
    }
    for (int i = 1; i <= sc.length - pc.length; i++) {
      char l = sc[i - 1];
      char r = sc[i + pc.length - 1];
      deltaSum -= Math.abs(delta[l]) + Math.abs(delta[r]);
      delta[l]--;
      delta[r]++;
      deltaSum += Math.abs(delta[l]) + Math.abs(delta[r]);
      if (deltaSum == 0) res.add(i);
    }
    return res;
  }
}
```

2025 code:

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> res = new ArrayList<>();

        if (s.length() < p.length()) {
            return res;
        }

        int[] freqS = new int[256];
        int[] freqP = new int[256];
        for (int i = 0; i < p.length(); i++) {
            freqS[s.charAt(i)]++;
            freqP[p.charAt(i)]++;
        }
        int[] delta = new int[256];
        int deltaSum = 0;
        for (int i = 0; i < 256; i++) {
            delta[i] = freqS[i] - freqP[i];
            deltaSum += Math.abs(delta[i]);
        }

        if (deltaSum == 0) {
            res.add(0);
        }
        for (int i = p.length(); i < s.length(); i++) {
            char oldChar = s.charAt(i - p.length());
            char newChar = s.charAt(i);
            deltaSum -= Math.abs(delta[oldChar]) + Math.abs(delta[newChar]);
            delta[oldChar]--;
            delta[newChar]++;
            deltaSum += Math.abs(delta[oldChar]) + Math.abs(delta[newChar]);
            if (deltaSum == 0) {
                res.add(i - p.length() + 1);
            }
        }
        return res;
    }
}
```