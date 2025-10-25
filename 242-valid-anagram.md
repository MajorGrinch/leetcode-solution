# 242. Valid Anagram

检查一下两个字符串是否是异构体，所谓异构体就是他俩可能不一样，但是组成的字母及其个数都是一样的。

那么其实就是检查一下各个字母出现的频率，并进行对比。很简单，这题主要是热身，熟悉一下什么是异构，为[438](438-Find-All-Anagrams.md)做准备。做完这题，赶紧就去做它。

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        int[] freqS = new int[256];
        int[] freqT = new int[256];
        for (int i = 0; i < s.length(); i++) {
            freqS[s.charAt(i)]++;
            freqT[t.charAt(i)]++;
        }
        for (int i = 0; i < freqS.length; i++) {
            if (freqS[i] != freqT[i]) {
                return false;
            }
        }
        return true;
    }
}
```