# 408. Valid Word Abbreviation

给定一个字符串和对应的缩略写法，判断是否匹配。

根据题意，一个字符串有很多种缩写，所以这里我们要仔细处理。采用双指针的形式，`i`来遍历字符串，`j`来遍历缩写。

当`j`遇到数字时，就得到后面整个数字num，然后让`i`对应地往后移动num位。当`j`遇到字符时，就看看`i`和`j`指向的字符是否一致。一致的话，各自走一步。不一致的话，立刻返回false。这样只要最后他俩同时遍历结束，就可以说字符串你和缩写匹配得上。

不过这题有个比较烦人的地方，就是数字不可以有leading zero，所以在判断`j`是否是数字的时候还需要判断是否是0。是0的话，就直接忽略，这样最后我们就可以返回false。

Time complexity: O(n)

Space complexity: O(1)

```java
class Solution {
  public boolean validWordAbbreviation(String word, String abbr) {
    int i = 0, j = 0;
    while (i < word.length() && j < abbr.length()) {
      if (Character.isDigit(abbr.charAt(j)) && abbr.charAt(j) != '0') {
        int count = 0;
        while (j < abbr.length() && Character.isDigit(abbr.charAt(j))) {
          count = count * 10 + abbr.charAt(j) - '0';
          j++;
        }
        i += count;
      } else {
        if (word.charAt(i) == abbr.charAt(j)) {
          i++;
          j++;
        } else {
          return false;
        }
      }
    }
    return i == word.length() && j == abbr.length();
  }
}
```