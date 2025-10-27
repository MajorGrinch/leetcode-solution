# Laicode 76. All Unique Characters I

## Single Frequent Integer Approach

和[laicode 77. All Unique Characters II](laicode-77-All-Unique-Chars-II.md)差不多，但是这题更简单。因为输入的字符只有26个小写字母，所以我们只要用一个`int`就可以记录下各个字母是否出现过。

Time complexity: O(n)

Space complexity: O(1)

```java
public class Solution {
  public boolean allUnique(String word) {
    int mask = 0;
    for (int i = 0; i < word.length(); i++) {
      char c = word.charAt(i);
      int num = 1 << (c - 'a');
      if ((mask & num) > 0) return false;
      mask |= num;
    }
    return true;
  }
}
```


## Frequent Array Approach

这题可以用一个长度为26的int数组来存放各个字母的出现次数，循环的时候只要当前字母之前出现过了那就返回false。

Time complexity: O(n)

Space complexity: O(1)

```java
public class Solution {
  public boolean allUnique(String word) {
    int[] freq = new int[26];
    for(int i = 0; i < word.length(); i++) {
      char c = word.charAt(i);
      if(freq[c - 'a'] != 0) {
        return false;
      }
      freq[c - 'a']++;
    }
    return true;
  }
}
```